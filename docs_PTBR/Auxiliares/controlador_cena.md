# Controlador de Cena

O controlador de cena é responsável por gerenciar a troca de mapas, controle do jogador entre cenas, sistema de pausa e cache de carregamento de cenas.

Ele atua como um ponto central da arquitetura do jogo para manipulação de estados globais relacionados ao mundo.

---

## Variáveis:

**world —**  
Nó principal onde os mapas são instanciados.

**mapa_atual —**  
Referência para o mapa atualmente carregado.

**jogador —**  
Referência ao jogador ativo na cena.

**menu_de_pause —**  
Interface de pausa do jogo.

**cache_cenas —**  
Dicionário utilizado para armazenar cenas já carregadas, evitando recarregamento.

**pausado —**  
Define se o jogo está pausado ou não.

---

## Funções

**inicializar —**  
Recebe o nó `world` e inicializa o controlador. Também tenta localizar o jogador automaticamente através do grupo `"Jogador"`.

---

**trocar_mapa —**  
Responsável por trocar o mapa atual.

Fluxo:
- Remove o jogador do mapa atual (caso necessário)
- Remove o mapa atual da memória
- Carrega a nova cena (com cache)
- Instancia e adiciona ao `world`
- Reinsere o jogador (opcional)
- Posiciona o jogador em um spawn específico

Parâmetros:
- `cena`: caminho da cena
- `usar_jogador`: define se o jogador será reutilizado
- `spawn`: nome do ponto de spawn

---

**obter_cena —**  
Carrega uma cena a partir do caminho informado.

- Utiliza cache para evitar múltiplos loads
- Armazena a cena após o primeiro carregamento

---

**get_jogador —**  
Busca o jogador na árvore de cena utilizando o grupo `"Jogador"`.

**Observação:**  
Depende da existência correta do grupo no jogador.

---

**erro_critico —**  
Exibe um erro e encerra o jogo imediatamente.

**Uso:**  
Situações onde o estado do jogo está inconsistente ou inválido.

---

**posicionar_jogador —**  
Move o jogador para um ponto de spawn dentro do mapa atual.

**Estrutura esperada:*`SpawnPoints/<nome_spawn>`*


---

**set_jogador_ativo —**  
Controla a visibilidade e processamento do jogador.

---

**registrar_menu —**  
Define o menu de pausa e garante que ele comece oculto.

---

**_ready —**  
Define o `process_mode` como `ALWAYS`, permitindo funcionamento mesmo com o jogo pausado.

---

**_input —**  
Escuta o input de pausa (`"Pausar"`) e alterna o estado do jogo.

---

**toggle_pause —**  
Alterna o estado de pausa:

- Atualiza variável interna
- Exibe/oculta o menu
- Define `get_tree().paused`

---

## Observações importantes

- O sistema depende do grupo `"Jogador"` para funcionamento correto.
- O cache de cenas não possui mecanismo de limpeza ainda!
- O método `erro_critico` encerra o jogo, devendo ser usado com cautela.