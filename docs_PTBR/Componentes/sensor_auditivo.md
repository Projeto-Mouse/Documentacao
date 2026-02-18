# Sensor Auditivo

Esse controlador é adicionado à entidade que pode ouvir ruídos.  
 Para utilizá-lo, crie um Node3D como filho da entidade e associe a ele o script do Sensor Auditivo.  
Ele é responsável por controlar o sistema de audição da entidade, permitindo que ela perceba ruídos gerados por outras entidades.

## Variáveis

### alcance_maximo -
Distância máxima que a entidade com esse sensor pode ouvir.

### mascara_oclusao -
Máscara usada no raycast para definir quais objetos bloqueiam a propagação do som.

## Funções

### _ready -
Define qual função será chamada quando o sinal do ControladorRuido for emitido.

### _ao_ruido_percebido -
Função chamada quando um ruído é emitido.  
Ela verifica se a distância entre quem escuta o ruído e sua origem é menor que o alcance efetivo.  
O alcance efetivo é calculado usando a intensidade do som e alcance_maximo.  
Se o som estiver fora do alcance, retorna false.  
Se estiver dentro, realiza um teste de oclusão.  
Se houver obstáculo no caminho, retorna false; caso contrário, retorna true.

### _verificar_oclusao -
Verifica a oclusão usando raycast.  
Testa se existe parede, chão ou qualquer objeto definido como oclusão entre a origem do ruído e quem escuta.  
true se colidir com algo  
false se o caminho estiver livre
