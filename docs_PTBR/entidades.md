# Sistema de Entidades e Inimigos

Este sistema define qualquer ser vivo do jogo e como os inimigos funcionam.

## Objetivo

- padronizar vida e dano
- evitar código repetido
- facilitar criação de novos inimigos
- manter comportamento previsível

## Hierarquia oficial

| Nível | Classe | Descrição |
|------|--------|----------|
| Base | Entidade | Base de tudo (herda de CharacterBody3D) |
| ↓ |  |  |
| Intermediária | Inimigo | Adiciona comportamento de ataque |
| ↓ |  |  |
| Específica | InimigoTerrestre | Anda no chão |
| Específica | InimigoVoador | Voa pelo ar |
| Específica | Outros futuros | Especializações futuras |

Nunca herdar direto de CharacterBody3D.

---

## Conceito de Entidade na Arquitetura

A Entidade é a base de qualquer ser vivo do jogo.  
Ela representa tudo que existe no mundo com vida, pode receber dano e possui presença física.  
A ideia dela é simples: centralizar tudo que é comum entre personagens e impedir que essa lógica fique espalhada pelo projeto.

A Entidade define apenas o essencial:

- existência física no mundo
- sistema de vida
- capacidade de receber dano

E propositalmente não define:

- comportamento
- inteligência
- decisões
- controle do jogador
- lógica específica de combate

Essas responsabilidades ficam nas classes que herdam dela.  
Essa separação mantém o sistema organizado e evita duplicação de código.  
Cada camada resolve um problema diferente: a Entidade garante a base, e as subclasses definem o comportamento.

Na arquitetura do jogo, a Entidade funciona como o ponto único de padronização para qualquer personagem vivo.

---

## Classe Base --- Entidade

A Entidade é a base de qualquer ser vivo: jogador, npc ou inimigo.  
Ela herda de CharacterBody3D, ou seja: é um corpo físico 3D que já possui movimento, colisão e gravidade do Godot.

### Ajustes pelo editor

@export permite ajustar valores direto no Godot:

- velocidade_base
- gravidade
- vida
- dano

### Movimento

Os personagens existem em 3D, mas se movem apenas para os lados.

```gdscript
velocity.z = 0
```

Movimento físico:

```gdscript
move_and_slide()
```

### Sistema de Vida

Função central:

```
computar_dano(dano_recebido)
```

A própria entidade:

- reduz vida
- verifica morte
- executa lógica de destruição

Toda regra de vida fica centralizada aqui.

---

## Classe Inimigo

Herda tudo da Entidade e adiciona comportamento de ataque.

### Inicialização

Executa quando o inimigo entra em cena:

```gdscript
if dano == 0:
    dano = 1.0
```

Evita ataques sem efeito.

### Funções estruturais

Funções com pass existem como estrutura para comportamento futuro:

- target() → definir alvo
- detectar_jogador_som() → detecção futura
- gerar_movimento_aleatorio() → comportamento ocioso

### Identificação de Alvo

O inimigo valida o objeto antes de causar dano.

Formas de identificar jogador:

```
alvo.name == "Jogador"
```

ou

```
alvo.is_in_group("Jogador")
```

Verificação segura:

```
alvo.has_method("computar_dano")
```

Aplicação de dano:

```
alvo.computar_dano(dano)
```

---

## Sistema de Dano por Contato

Existem duas formas de detecção.

### 1 --- Colisão Física

Função:

```
verificar_dano_contato()
```

Fluxo:

- verifica quantas colisões ocorreram
- percorre cada colisão
- obtém o objeto real
- tenta aplicar dano

Funções usadas:

```
get_slide_collision_count()
get_slide_collision(i)
get_collider()
```

### 2 --- Área Fantasma (Area3D)

Função conectada ao sinal:

```
_on_body_entered(body)
```

Não bloqueia movimento físico.

Uso recomendado:

- zonas de ataque
- projéteis
- sensores

---

## Fluxo Completo de Combate

- inimigo se move
- encosta em algo
- sistema pega o objeto
- valida alvo
- chama computar_dano
- alvo perde vida

---

## InimigoTerrestre

Inimigo que anda no chão e muda direção com o tempo.

### Gravidade manual

```gdscript
if not is_on_floor():
    velocity.y -= gravidade * delta
```

### Sistema de decisão

Timer regressivo:

```
tempo_mudanca_direcao -= delta
```

Novo tempo:

```
randf_range(1.0, 4.0)
```

### Escolha de direção

Probabilidades:

- < 0.33 → esquerda
- < 0.66 → direita
- else → parado

---

## InimigoVoador

Inimigo aéreo com máquina de estados.  
Não sofre gravidade constante.

### Estados

```
enum EstadoVoo { VOANDO, POUSANDO }
```

### Limite de altura

Impede que o inimigo saia do mapa.

Se ultrapassar altura máxima:

- cancela subida
- limita posição

### Sistema de decisão

A cada intervalo aleatório:

- define nova direção
- define estado

Probabilidade:

- 30% pousar
- 70% voar

### Estado VOANDO

- ignora gravidade
- movimento livre em X e Y

### Estado POUSANDO

```gdscript
velocity.x = 0
velocity.y -= gravidade * 0.5
```

Desce suavemente.

---

## Guia Oficial --- Criar Novos Inimigos

Todo inimigo novo deve herdar de Inimigo.

### Hierarquia obrigatória

Entidade → Inimigo → NovoInimigo

A Entidade já resolve:

- vida
- dano
- movimento
- colisão

### Passo a passo

1 --- Criar script

```
extends Inimigo
```

2 --- Definir comportamento

Todo inimigo deve decidir:

- sofre gravidade?
- como escolhe direção?
- possui estados?

3 --- Estrutura padrão de física

```gdscript
func _physics_process(delta):
    lógica própria
    move_and_slide()
    verificar_dano_contato()
```

Regras:

- sempre mover com move_and_slide
- sempre verificar colisão depois
- não alterar computar_dano

4 --- Sistema de decisão recomendado

```
tempo_decisao -= delta
if tempo_decisao <= 0:
    tempo_decisao = randf_range(min, max)
    tomar_decisao()
```

5 --- Uso de estados (recomendado)

```
enum Estado { IDLE, PERSEGUINDO, ATACANDO }
var estado_atual = Estado.IDLE
```

---

## Regras de Segurança

- sempre validar alvo antes de causar dano
- nunca manipular vida diretamente
- sempre usar computar_dano
- não duplicar lógica da Entidade

Exemplo correto:

```
if alvo.has_method("computar_dano"):
    alvo.computar_dano(dano)
```

---

## Template Oficial de Novo Inimigo

```gdscript
extends Inimigo

var tempo_decisao = 0
var direcao = 0

func _physics_process(delta):
    aplicar_gravidade(delta)
    tempo_decisao -= delta
    if tempo_decisao <= 0:
        tempo_decisao = randf_range(1.0, 3.0)
        escolher_comportamento()
    aplicar_movimento()
    move_and_slide()
    verificar_dano_contato()

func aplicar_gravidade(delta):
    pass

func escolher_comportamento():
    pass

func aplicar_movimento():
    pass
```

---

## Resumo Final

A Entidade cuida da base: vida, dano e movimento. O Inimigo usa essa base e adiciona ataque e interação com o jogador. As especializações só definem como se movem e se comportam.

Cada camada tem uma responsabilidade clara. Isso mantém o sistema organizado, evita repetição de código e facilita criar novos inimigos sem quebrar o que já funciona.

Se um inimigo novo respeitar essa estrutura, ele já nasce compatível com todo o sistema.

