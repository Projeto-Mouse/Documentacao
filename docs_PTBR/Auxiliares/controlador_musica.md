# Controlador Música

O controlador de música é responsável por fazer o controle da música durante a gameplay, como troca, setar volume ( futuramente ), parar música, tocar com loop ( futuramente ).

## Variáveis:

**duracao_fade -**  
Diz respeito ao tempo de duração do fade.

**volume_alvo_db -**  
O volume alvo para a função fade_in

**reprodutor -**  
Objeto que realmente faz o controle nas funções.

---

## Funções:

**tocar_musica -**  
Para a música que possivelmente esteja tocando e começa a tocar a outra.

**trocar_musica -**  
Faz o fade_out da música que está tocando e começa outra com fade_in. Diferente da de tocar_musica que se estiver tocando uma ela para na hora.

**parar_musica -**  
Para a música que está tocando.

**fade_out -**  
Usa o tween para fazer uma animação do volume que está até o volume -80.0dbs e para a musica.

**fade_in -**  
O que o fade_in faz é setar o volume para -80.0dbs e subi-lo animado com tween até o valor_alvo_db.
