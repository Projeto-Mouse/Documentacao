# SISTEMA DE ITENS E COMO CRIAR UM

## 1 - Introdução

A ideia aqui é apresentar a estrutura atual que estamos usando de itens e como criar um item novo, caso você precise, e vai precisar.

## 2 - Estrutura de pastas e o que cada uma faz.

```
📁 Itens/
├── 📁 Cenas/
│   ├── 📁 CenasMapa/

│   └── 📁 CenasMaoJogador/

├── 📁 Equipamentos/
│   ├── 📁 Armas/

│   ├── 📁 Armaduras/
│   │
│   ├── 📁 Ferramentas/

├── 📄 ItemData.gd
└── 📄 ItemMundo.gd
```

Por enquanto a estrutura de pastas está assim, e acho que está em um bom caminho.

### 2.1 - O que cada pasta/arquivo faz

```
📁 Itens/
```

Essa é a pasta principal é, obviamente a pasta que vamos guardar tudo que tem haver com itens.

```
├── 📁 Cenas/
│   ├── 📁 CenasMapa/

│   └── 📁 CenasMaoJogador/
```

Essa estrutura de pasta vão guardar as cenas que temos.

Isso é:

CenasMapa - É a cena do item no mapa. Ela guarda area3d, colisionshape ( se tiver ), o script ItemMundo, que sera abordado a frente.

CenasMaoJogador - É onde vamos guardar as cenas do mesmo item, só que a cena que é um Node3D com um Sprite3D e possivelmente algum script do item, que controla algo que o item fassa, por exemplo a tocha que tem a Omnilight3D e o script da luz, para fazer piscar.

Esses nós que fazem parte do item, os scripts deles e que funcionam na mao do jogador, a luz piscar funciona na mao do jogador, ficam na cena que fica nessa pasta.

Resumindo os itens que ficam no mundo vao ter a cena na pasta itens mundo, quando pegarmos o item do mundo vamos instanciar a cena item mao ( que fica na outra pasta ) na mao do jogador.

Sempre que pegarmos o item do inventario e a cena item mão que vamos instanciar na mão.

```
├── 📁 Equipamentos/
│   ├── 📁 Armas/

│   ├── 📁 Armaduras/
│   │
│   ├── 📁 Ferramentas/
```

Essa parte é onde vão ficar as datas de cada item.


Primeiro vamos definir o que é data.

Data é um script que não tem métodos ele serve apenas para guardar variaveis e com ele geramos o .tres

Que é o arquivo em que colocamos esses dados, que são os valores dessas variaveis, pelo inspetor.

Isso também vai ser explicado mais a frente.

Nessas pastas vamos ter os scripts que precisamos para cada item.

Ou seja dentro de Ferramentas vamos encontrar a pasta Tocha e dentro vamos ter:

* Script da luz, ou algum script que controle uma propriedade do item. EX: Armas que aplicam veneno vão ter um script que lê o ataque do jogador e aplica o veneno.
Esse script vai ser posto no Node3D do item na cena que fica na pasta CenasMaoJogador.

* Script TochaData.gd que guarda os dados da tocha, com variaveis a mais que ItemData e FerramentaData tem. EX: Tempo em que a tocha fica acessa.
Esse script vai ser posto em uma parte do inspetor quando clicamos no Node3D do item.

* tocha_data.tres que é a instancia do TochaData.gd. É de fato o arquivo que botamos no inspetor no Node3D do item e permite setarmos valores pelo inspetor para as variaveis que a tocha tem.


E assim se segue. Armas tem o script ArmaData.gd e pastas de armas.

Então até agora cada item tem: 

* Uma pasta para cenas dos itens que vamos por no mundo

* Uma pasta para os mesmos itens, mas com as cenas que vamos instanciar na mão do jogador.

* Uma pasta para os scripts e script de data do item, além do .tres.

### 2.2 Script ItemData e ItemMundo.

O script ItemData é o script que tem as variáveis de que todos os itens do jogo tem.

Todos os arquivos de data de cada item vai extender desse, ou de algum filho dele.

Por exemplo TochaData extende de FerramentaData que extende de EquipamentoData que extende de ItemData.

O script ItemMundo é o script que botamos no Node3D do item na cena do item que instanciamos no mundo, para o jogador pegar.

Ele adiciona o item ao grupo interagivel e verifica se o item tem uma Area3D.

Se ele tem ele cria um método que troca a variável item_da_area_atual do jogador para que possamos pegar aquele item e guardar no inventário, instanciar na mão e etc...

** OBS: Possivelmente esse ItemMundo vai ser refatorado para ItemMundoInteragivel, pois possivelmente iremos precisar de um script para itens que estão no mundo, mas não são interagiveis **

Além disso ItemMundo tem uma variavel em @export ( para mudarmos pelo inspetor ) que guarda o .tres do item.

E ItemData ( que ira virá um .tres ) tem uma variável que guarda a cena_3d do item, que é a cena que vamos usar na mão do jogador e que está na pasta CenasMaoJogador.

cena_3d também e um @export ou seja botamos pelo inspetor.

## 3 - Como criar um item novo.

Após criamos o sprite do item vamos colocar na pasta sprite.

** OBS: Essa pasta provavelmente vai ser refatorada mais para frente para guardamos os sprites que vamos usar quando o item esta no mundo, quando está no inventário, quando está na mão e etc... **

Assim vamos na pasta Itens e vamos ir até a pasta que aquele item representa.

Vamos supor uma tesoura ela é uma ferramenta.

Então vamos até a pasta ferramenta e criar uma pasta nova chamada Tesoura.

Lá vamos ter TesouraData.gd e vamos gerar o tesoura_data.tres usando o TesouraData.gd

E também nessa mesma pasta podemos ter scripts de coisas que Tesoura faz, por exemplo um script que tem um método que verifica se a tesoura tem um tempo sem ser usada e corta mais cabelo por isso.

** OBS: Lembrando que isso é so um exemplo **

Agora que fizemos a pasta Tesoura dentro de ferramenta e botamos os scripts de data e possiveis scripts de regras de negócio, além de ter gerado o .tres, vamos na pasta Cenas/CenasMundo e vamos criar uma nova Cena3D.

Essa nova Cena3D vai ter um Node3D, um Sprite3D que vai ser o sprite da tesoura e algumas coisas a mais que talvez queremos botar.

No Node3d do Item vamos botar o Script ItemMundo e no inspetor, na parte Item Data, vamos por o tesoura_data.tres que geramos usando o TesouraData.

Depois disso vamos na pasta Cenas/CenasMaoJogador e vamos criar uma Cena3D que dessa vez vai ter um Node3D, um Sprite3D e talvez mais algumas coisas.

No Node3D dessa cena vamos adicionar o possivel script para esse item, um exemplo da tesoura é aquele que falei que verifica se a tesoura tem um tempo sem ser usada e corta mais cabelo.

Agora:

Vamos la no tesoura_data.tres na pasta Ferramentas/Tesoura e vamos clicar duas vezes, abrindo-a no inspetor.

No inspetor poderemos preencher todos os dados como: Nome, Durabilidade, Raridade, Preço e etc...

Além disso vamos na parte cena_3d e vamos adicionar a cena que esta na pasta Cenas/CenasMaoJogador.

E pronto agora é so instanciar a cena que esta em Cenas/CenasMapa no mapa e temos o item la pronto para ser pego pelo jogador.

## 4 - Algumas imagens

