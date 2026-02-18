# Controlador Iluminação

O controlador de iluminação é responsável por gerenciar o ciclo dia/noite do jogo, bem como o movimento dos corpos celestes no céu.

## Variáveis:

**camera_alvo —**  
A câmera principal da cena, usada como referência para a luz spot.

**luz_direcional —**  
Luz que representa o sol.

**luz_spot —**  
Luz local que acompanha a câmera.

**ambiente_mundial —**  
Controla o ambiente global da cena.

**offset_spot —**  
Deslocamento da luz spot em relação à câmera.

**altura_spot_z —**  
Altura da luz spot.

**duracao_ciclo_segundos —**  
Duração total de um ciclo em segundos.

**escala_tempo —**  
Multiplicador da velocidade do tempo.

**tempo_atual —**  
Tempo atual dentro do ciclo.

**luz_lua —**  
Luz que representa a lua.

**_debug_acelerado —**  
Flag interna para verificar se o modo debug está ativo.

**fase_atual —**  
Fase atual do ciclo (dia, anoitecer, noite, amanhecer).

---

## Cores do ciclo

**cor_dia -**  
ffffff (branco).

**cor_anoitecer -**  
ff00ff (magenta).

**cor_noite -**  
8da3b8 (Cinzento).

**cor_amanhecer -**  
ffaa00 (Laranja)

---

## Fases do dia

Enumeração que define os estados do ciclo: Dia, Anoitecer, Noite e Amanhecer.

---

## Funções

**_ready -**  
Configura a cena, definindo as configurações da luz_lua e fazendo um warning caso não tenha camera_alvo ou luz_direcional.

**atualizar_tempo -**  
Atualiza tempo_atual com base em delta e escala_tempo. Detecta a passagem de minutos para debug e reinicia o ciclo ao atingir a duração máxima.

**atualizar_ciclo_iluminacao -**  
Atualiza o ciclo com base no tempo atual, alterando o estado e realizando transições suaves de cor e energia das luzes usando interpolação (lerp).

**atualizar_rotacao_corpos_celestes -**  
Move os corpos celestes pelo céu, simulando rotação real. Os ângulos variam de -10° a -170°.

**atualizar_posicao_spot -**  
Atualiza a posição da luz spot com base na câmera.
