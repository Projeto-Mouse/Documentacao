# Controlador Ruído

O Controlador Ruído serve para controlar a geração de ruído emitido pelo jogador. Ele pega e emite um sinal com a posição e intensidade do ruído. Além do mais, ele trata do desenho de debug do ruído.

## Sinal:

Emite um sinal chamado ruido_gerado que recebe como parâmetro de emissão a posição e a intensidade do ruído gerado.

---

## FUNÇÕES:

**emitir_ruido -**  
Chamada pelo objeto que está emitindo o ruído. Pega sua posição, intensidade, se está com o debug apertado, e o pai que emitiu e chama um sinal emitido. ( Para mais detalhes entre na documentação do sensor auditivo. )

**_desenhar_debug -**  
Desenha uma esfera que sai do centro de quem chamou o debug e se projeta para cima. Todos dentro dessa esfera escutam o som.
