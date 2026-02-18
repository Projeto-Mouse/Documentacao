# Script Objeto Escalável

Esse script é posto em objetos escaláveis como cipó para controlar quando o jogador entra e quando sai. Além de chamar uma função do jogador que setar para o estado está em escalável ou não está.

## Funções

### _ready -
Faz um bind para chamar a função gerenciar_area_escalavel com true ou false, dependendo se o jogador entrou ou saiu da area3d do objeto com esse script.

### gerenciar_area_escalavel -
Verifica se o corpo que entrou é do grupo jogador, e se for chama a função que está no jogador ( setar_esta_em_escalavel ) para trocar o estado dele de algum outro para o estado escalando.
