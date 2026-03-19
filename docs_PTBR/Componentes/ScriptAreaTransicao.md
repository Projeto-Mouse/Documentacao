# Área de Transição

A área de transição é um componente responsável por detectar a entrada do jogador em uma região específica e acionar a troca de mapa automaticamente.

Esse script deve ser associado a um `Area3D` dentro da cena. Quando o jogador entra na área, o controlador de cena é acionado para carregar um novo mapa.

---

## Variáveis

### caminho_cena —
Caminho da cena que será carregada ao entrar na área de transição.

### usar_jogador —
Define se o jogador atual será reutilizado na nova cena.

### spawn —
Nome do ponto de spawn onde o jogador será posicionado no novo mapa.

### portas —
Controla se a área de transição está ativa.  
Inicialmente desativada para evitar ativações indesejadas ao iniciar a cena.

---

## Funções

### _ready —
Conecta o sinal `body_entered` à função de gerenciamento da transição.  
Também adiciona um pequeno atraso antes de ativar a área, evitando disparos imediatos ao carregar a cena.

---

### gerenciar_area_transicao —
Função chamada quando um corpo entra na área.

Fluxo:
- Verifica se a área está ativa (`portas`)
- Confirma se o corpo pertence ao grupo `"Jogador"`
- Verifica se o caminho da cena é válido
- Aciona a troca de mapa através do `ControladorCena`

Caso o caminho da cena esteja vazio, exibe uma mensagem de erro no console.
