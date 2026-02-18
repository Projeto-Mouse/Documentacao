# Arquitetura do Sistema de Inimigos

## 1. Visão Geral
Este documento descreve a arquitetura do Sistema de Inimigos, centralizando todas as informações relacionadas à hierarquia de classes, responsabilidades, características e comportamentos dos inimigos do jogo.  
O objetivo é garantir clareza e facilidade de extensão do sistema.

## 2. Hierarquia de Classes

```text
Entidade
└── Inimigo
   ├── InimigoTerrestre
   └── InimigoVoador
```

## 3. Entidade
Classe base do projeto para todas as entidades do jogo.

### Responsabilidades
- Representar qualquer entidade ativa no jogo
- Centralizar atributos e comportamentos comuns
- Servir como base para todas as especializações

Classe representada por: Entidade  
 Entidade 

## 4. Inimigo
Classe base responsável por todos os inimigos do jogo.  
 Extende a classe Entidade.  
Inimigo e Entidade 

### Responsabilidades
- Aplicar dano ao jogador
- Detectar colisões
- Definir comportamentos comuns a todos os inimigos
- Centralizar a lógica compartilhada

### Características Gerais
Características herdadas por todos os inimigos do jogo:
- Possuem pontos de vida
- Possuem lógica de movimentação própria
- Aplicam dano ao entrar em contato com o jogador
- Compartilham regras comuns de colisão
- Permitem especialização sem duplicação de código

## 5. InimigoTerrestre
Especialização da classe Inimigo.  
 InimigoTerrestre e Inimigo 

### Descrição
Representa inimigos que se movimentam pelo chão e são afetados pela gravidade e pelas superfícies do cenário.

### Características
- Utilizam gravidade
- Movimento predominantemente horizontal
- Dependem de superfícies sólidas
- Mudam de direção de forma aleatória

### Comportamentos
- Andam para a esquerda e para a direita
- Ajustam o movimento conforme colisões com o cenário
- Podem permanecer parados por intervalos de tempo
- Respeitam os limites físicos do ambiente

## 6. InimigoVoador
Especialização da classe Inimigo.  
 InimigoVoador 

### Descrição
Representa inimigos que se movimentam livremente pelo ar e não dependem do chão.

### Características
- Ignoram o chão
- Possuem estados de voo e pouso
- Possuem limite máximo de altura

### Comportamentos
- Alternam entre voo e pouso
- Ajustam a direção de forma aleatória
- Movimentam-se nos eixos horizontal e vertical
- Mantêm um limite máximo de deslocamento vertical

## 7. Criação de Novos Inimigos
Para criar um novo tipo de inimigo, deve-se:

- Criar uma nova classe que estenda Inimigo ou uma de suas especializações
 (InimigoTerrestre ou InimigoVoador)
- Implementar o método gerar_movimento(delta)
- Não sobrescrever métodos responsáveis por:
  - Aplicação de dano
  - Detecção de colisão
- Ajustar apenas variáveis de comportamento, como:
  - Velocidade
  - Dano
  - Tempo de movimentação
- Testar o inimigo de forma isolada antes da integração ao jogo

## 8. Observações Finais
Novos inimigos podem variar a partir das especificações já existentes, desde que respeitem a arquitetura definida e não alterem comportamentos comuns centralizados na classe Inimigo.

Essa abordagem garante:

- Reutilização de código
- Facilidade de manutenção
- Escalabilidade do sistema

