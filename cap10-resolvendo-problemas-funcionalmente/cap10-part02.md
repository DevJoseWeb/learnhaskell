De Heathrow para Londres
========================

Nosso próximo problema é esse: o avião onde você estava acabou de aterrissar na Inglaterra e você aluga um carro. Você tem uma reunião logo logo e tem que ir do Heathrow até Londres o mais rápido que puder (mas seguramente!).

Há duas estradas principais indo de Heathrow para Londres e há um número de estradas regionais cruzando-as. Leva-se uma quantidade de tempo fixa para se viajar de um cruzamento para outro. Depende de você encontrar o caminho ideal a se tomar para que você chegue a Londres o mais rápido possível! Você começa do lado esquerdo e pode tanto cruzar para a outra estrada principal ou seguir em frente.

Como você pode ver nesta figura, o caminho mais curto de Heathrow para Londres nesse caso é começar pela estrada principal B, cruzar, seguir em frente em A, cruzar novamente e então ir em frente duas vezes em B. Se tomarmos esse caminho, levaremos 75 minutos. Se tivéssemos escolhido qualquer outro caminho, levaria mais tempo que isso.
Nosso trabalho é fazer um programa que recebe uma entrada que representa o sistema de entradas e dá como saída qual é o caminho mais curto atravessando-o. Aqui está como a entrada se pareceria nesse caso:

Para se entender o arquivo de entrada, leia-o de três em três e divida mentalmente o sistema de estradas em seções. Cada seção é composta de uma estrada A, uma estrada B e uma estrada cruzando-as. Para que a entrada elegantemente divida-se em grupos de três, dizemos que há uma última seção de cruzamento que demora 0 minutos para ser atravessada. Isso é porque não nos importamos em que parte de Londres nós chegamos, contanto que estejamos em Londres.
Assim como fizemos quando resolvemos o problema da calculadora RPN, vamos resolver esse problema em três passos:

- Esqueça Haskell por um minuto e pense em como nós resolveríamos esse problema na mão.
- Pense sobre como vamos representar nossos dados em Haskell.
- Descubra como operar sobre esses dados em Haskell para que produzamos a solução.

Na seção da calculadora RPN, primeiro nós descobrimos que quando estávamos calculando uma expressão na mão, tínhamos de manter uma pilha em nossas mentes e então passar um item de cada vez pela expressão. Nós decidimos usar uma lista de strings para representar nossa expressão. Finalmente, usamos uma dobra à esquerda para andar sobre a lista de strings enquanto mantivemos uma pilha para produzir uma solução.

Certo, então como nós poderíamos encontrar o caminho mais curto de Heathrow para Londres na mão? Bem, podemos meramente meio que olhar para a figura inteira e tentar adivinhar qual é o caminho mais curto e esperançosamente daremos um palpite que estará correto. Essa solução funciona para entradas muito pequenas, mas e se nós tivermos uma estrada que tenha 10.000 seções? Caramba! Nós também não poderemos dizer se a nossa solução é a ideal, podemos apenas meio que dizer que estamos bem certos disso.

Essa então não é uma solução boa. Aqui está uma figura simplificada do nosso sistema de estradas:

Pronto, você consegue descobrir qual é o caminho mais curto para o primeiro cruzamento (o primeiro ponto azul em A, marcado <i>A1</i>) na estrada A? É bem trivial. Nós simplesmente vemos se é mais curto seguir diretamente em A ou se é mais curto ir diretamente em B e então cruzar. Óbviamente, é mais barato seguir em frente via B e então cruzar porque isso leva 40 minutos, enquanto que ir direto por A leva 50 minutos. E quanto ao cruzamento <i>B1</i>? A mesma coisa. Vemos que é muito mais barato simplesmente ir diretamente via B (sujeitando-se ao custo de 10 minutos), porque ir por A e então cruzar levaria completos 80 minutos!

Agora sabemos qual é o caminho mais curto para <i>A1</i> (ir via B e então cruzar, então diremos que isso é [code]B, C[/code] com o custo de 40) e sabemos qual é o caminho mais curto para <i>B1</i> (vá diretamente via B, então é apenas [code]B[/code], custando 10). No fim das contas, esse conhecimento nos ajuda de alguma forma se quisermos saber qual o caminho mais barato para o próximo cruzamento em ambas estradas principais? Caramba, com certeza sim!
Vamos ver qual seria o caminho mais curto para <i>A2</i>. Para chegar em <i>A2</i>, nós vamos ou diretamente de <i>A1</i> para <i>A2</i> ou nós vamos em frente de <i>B1</i> e então cruzamos (lembre-se, podemos apenas ou seguir em frente ou cruzar para o outro lado). E porque conhecemos o custo para <i>A1</i> e <i>B1</i>, podemos facilmente descobrir qual é o melhor caminho para <i>A2</i>. Custa 40 para chegar em <i>A1</i> e então 5 para ir de <i>A1</i> para <i>A2</i>, então isso é [code]B, C, A[/code] por um preço de 45. Custa apenas 10 para se chegar em <i>B1</i>, mas então ter-se-ia de gastar adicionais 110 minutos para ir até <i>B2</i> e então cruzar para o outro lado! Então obviamente, o caminho mais barato para <i>B2</i> é [code]B, C, A[/code]. Do mesmo jeito, o caminho mais barato para <i>B</i> é seguir em frente de <i>A1</i> e então cruzar para o outro lado.

<em>Talvez você esteja se perguntando</em>: mas e quanto a chegar até <i>A2</i> por primeiro cruzar em <i>B1</i> e então seguir adiante? Bem, já cobrimos de <i>B1</i> até <i>A1</i> quando estávamos procurando pelo melhor caminho até <i>A1</i>, daí não temos que levar isso em conta no próximo passo.

Agora que temos o melhor caminho para <i>A2</i> e <i>B2</i>, podemos repetir isso infinitamente até chegarmos no fim. Assim que tivermos conseguido os melhores caminhos para <i>A4</i> e <i>B4</i>, o que for mais barato é o caminho ótimo!
Então em essência, para a segunda seção, apenas repetimos o passo que fizemos na primeira, só que levamos em conta quais foram os melhores caminhos anteriores em A e B. Poderíamos dizer que também levamos em consideração os melhores caminhos em A e B no primeiro passo, só que eles eram ambos caminhos vazios com um custo de 0.
Aqui está um resumo. Para se conseguir o melhor caminho de Heathrow para Londres, fazemos isso primeiro: vemos qual é o melhor caminho para o próximo cruzamento na estrada principal A. As duas opções são seguir diretamente em frente ou começar na estrada oposta, seguir em frente e então cruzar. Lembramos o custo e o caminho. Usamos o mesmo método para ver qual o melhor caminho para o próximo cruzamento na estrada principal B e não esquecemos disso. Daí, vemos se o caminho para o próximo cruzamento na estrada principal A é mais barato se formos a partir do cruzamento anterior em A ou se formos a partir do cruzamento anterior em B e então cruzar. Lembramos do caminho mais barato e então fazemos o mesmo para o cruzamento oposto a esse. Fazemos isso para seção até chegarmos no fim. Uma vez que tenhamos chegado no fim, o mais barato dos dois caminhos que nós tivermos é nosso caminho ideal.

Então em essência, nós mantemos um caminho mais curto na estrada A e um caminho mais curto na estrada B e quando chegamos no fim, o mais curto desses dois é nosso caminho. Nós agora sabemos como encontrar o caminho mais curto na mão. Se você tivesse tempo o suficiente, papel e lápis, você poderia encontrar o caminho mais curto através de um sistema de estradas com qualquer número de seções.

Próximo passo! Como representamos esse sistema de estradas com os tipos de dados de Haskell? Uma forma é pensar nos pontos iniciais e cruzamentos como nodos de um grafo que apontam pra outros cruzamentos. Se imaginarmos que os pontos iniciais na verdade apontam de um pro outro com uma estrada quem tenha o tamanho 1, vemos que todo cruzamento (ou nodo) aponta para o nodo do outro lado e também para o próximo no seu lado. Exceto pelos últimos nodos, eles apenas apontam para o outro lado.


Um nodo é tanto um nodo normal, que tem informação sobre a estrada que leva para a outra estrada principal e sobre a estrada que leva para o próximo nodo, ou é um nodo final, que só tem informação sobre a estrada para a outra estrada principal. Uma estrada mantém informação sobre quão longa ela é e para qual nodo ela aponta. Por exemplo, a primeira parte da estrada na estrada principal A seria [code]Road 50 a1[/code] onde [code]a1[/code] seria um nodo [code]Node x y[/code], onde [code]x[/code] e [code]y[/code] são estradas que apontam para <i>B1</i> e <i>A1</i>.

Outra forma seria usar [code]Maybe[/code] para as partes da estrada que apontam adiante. Cada nodo tem uma parte que aponta para para a estrada oposta, mas apenas aqueles nodos que não são os finais têm partes de estradas que apontam adiante.


Essa é uma forma aceitável para se representar um sistema de estradas em Haskell e poderíamos certamente resolver esse problema com ela, mas talvez nós pudéssemos pensar em algo mais simples? Se pensarmos voltando para nossa solução feita à mão, nós sempre apenas checamos os tamanhos de três partes de estrada por vez: a parte da estrada na estrada A, sua parte oposta na estrada B e a parte C, que toca nessas duas partes e as conecta. Quando estávamos procurando pelo menor caminho até <i>A1</i> e <i>B1</i>, tínhamos apenas que lidar com os tamanhos das três primeiras partes, as quais tinham os tamanhos de 50, 10 e 30. Vamos chamar isso de uma seção. Então o sistema de estradas que usamos para esse exemplo pode ser facilmente representado como quatro seções:  [code]50, 10, 30[/code], [code]5, 90, 20[/code], [code]40, 2, 25[/code], e [code]10, 8, 0[/code].
É sempre bom manter nossos tipos de dados o mais simples possível, apesar de não ser tão simples!



Isso é bem perfeito! É simples assim e eu sinto que isso funcionará perfeitamente na implementação de nossa solução. [code]Section[/code] é um tipo algébrico simples que contem três inteiros para os tamanhos das três partes das estradas. Nós também introduzimos um tipo sinônimo, dizendo que [code]RoasSystem[/code] é uma lista de seções.


Também poderíamos usar uma tripla de [code](Int, Int, Int)[/code] para representar uma seção de estrada. Usar tuplas em vez de nossos próprios tipos é bom para algumas pequenas coisas localizadas, mas é geralmente melhor fazer um novo tipo para coisas desse tipo. Isso dá ao sistema de tipos mais informação sobre o que é o quê. Podemos usar [code](Int, Int, Int)[/code] para representar uma seção de estrada ou um vetor em um espaço 3D e podemos operar nesses dois, mas isso nos permite misturar as coisas. Se usarmos os tipos de dados [code]Section[/code] e [code]Vector[/code], então não podemos acidentalmente adicionar um vetor com uma secção de um sistema de estradas.

Nosso sistema de estradas de Heathrow para Londres agora pode ser representado dessa forma:



Tudo que precisamos agora é implementar em Haskell a solução que inventamos anteriormente. Qual deveria ser a declaração de tipo para uma função que calcula o caminho mais curto para um dado sistema de estradas qualquer? Ela tem que receber o sistema de entradas como parâmetro e retornar um caminho. Vamos representar um caminho também como uma lista. Vamos introduzir um tipo [code]Label[/code] que é apenas uma enumeração tanto de [code]A[/code], [code]B[/code] ou [code]C[/code]. Vamos também criar um tipo sinônimo: [code]Path[/code].



Nossa função, nós a chamaremos de [code]optimalPath[/code], deveria então ter uma declaração de tipo de [code]optimalPath :: RoadSystem -&gt; Path[/code]. Se chamada com o sistema de estradas [code]heathrowToLondon[/code], deveria retornar o seguinte caminho:


Vamos ter que passar pela lista das seções da esquerda para a direita e manter o caminho ideal em A e o caminho ideal em B durante a passagem. Vamos acumular o melhor caminho a medida que passamos pela lista, da esquerda para a direita. O que isso parece? Ding, ding, ding! Exatamente, UMA DOBRA PARA A ESQUERDA!
Quando estávamos fazendo a solução na mão, havia um passo que nós repetimos várias e várias vezes.  Envolvia checar os novos caminhos ideias em A e B. Por exemplo, no começo os caminhos ótimos eram [code][][/code] e [code][][/code]  para A e B respectivamente. Examinamos a seção [code]Section 50 10 30[/code] e concluímos que o novo caminho ideal para <i>A1</i> era [code][(B, 10), (C, 30)][/code] e o caminho ideal para <i>B1</i> era [code][(B, 10)][/code]. Se você olhar para esse passo como uma função, ela leva um par de caminhos e uma seção e produz um novo par de caminhos. O tipo é [code](Path, Path) -&gt; Section -&gt; (Path, Path)[/code]. Vamos em frente implementar essa função, porque ela está destinada a ser útl.

<em>Dica:</em> vai ser útil porque [code](Path, Path) -&gt; Section -&gt; (Path, Path)[/code]  pode ser usada como a função binária de uma dobra à esquerda, que tem que ser do tipo [code]a -&gt; b -&gt; a[/code]


O que está acontecendo aqui? Primeiro, calcular o preço ideal na estrada A baseado no melhor até agora em A e fazemos o mesmo para B. Fazemos [code]sum $ map snd pathA[/code], então se  [code]pathA[/code] for algo do tipo [code][(A,100),(C,20)][/code], [code]priceA[/code] torna-se [code]120[/code]. [code]forwardPriceToA[/code] é o preço que pagaríamos se fossemos para o próximo cruzamento em A se fossemos pra lá diretamente do cruzamento anterior em A. Isso iguala o melhor preço do nosso A anterior, mais o tamanho da parte de A da seção atual. [code]crossPriceToA[/code] é o preço que pagaríamos se nós fossemos para o próximo A indo em frente do B anterior e então cruzando. É o melhor preço para o B anterior até agora mais o tamanho de B da seção mais o tamanho de C da seção. Determinamos [code]forwardPriceToB[/code] e [code]crossPriceToB[/code] da mesma maneira.

Agora que sabemos qual é o melhor caminho para A e para B, precisamos apenas criar os novos caminhos para A e B baseados nisso. Se for mais barato ir para A apenas por seguir em frente, setamos [code]newPathToA[/code] para ser [code](A,a):pathA[/code]. Basicamente nós prefixamos a  [code]Label[/code] [code]A[/code] e o tamanho da seção [code]a[/code] para o caminho ideal em A até agora. Basicamente, dizemos que o melhor caminho para o próximo cruzamento A é o caminho para o cruzamento A anterior e então uma seção adiante via A. Lembre-se, [code]A[/code] é apenas um rótulo, enquanto que [code]a[/code] tem o tipo de [code]Int[/code]. Por que nós prefixamos em vez de fazer [code]pathA ++ [(A,a)][/code]? Bem, adicionar um elemento no início de uma lista (também conhecido como “consing”) é muito mais rápido que adicioná-lo no fim. Isso significa que o caminho estará invertido para o lado errado quando dobrarmos sobre uma lista com essa função, mas é fácil inverter a lista depois. Se for mais fácil chegar no próximo cruzamento A indo direto pela estrada B e então cruzando, então [code]newPathToA[/code] é o antigo caminho para B a partir do qual segue-se em frente e cruza para A. Fazemos a mesma coisa para [code]newPathToB[/code], só que tudo agora está espelhado.

Finalmente, retornamos [code]newPathToA[/code] e [code]newPathToB[/code] em um par.
Vamos rodar essa função na primeira seção de [code]heathrowToLondon[/code]. Porque é a primeira seção, os melhores caminhos nos parâmetros A e B será uma par de listas vazias.


Lembre-se, os caminhos estão invertidos, então leia-os da direita para a esquerda. Disso, podemos ler que o melhor caminho para o próximo A é começar de B e então cruzar para A e que o melhor caminho para o próximo B é apenas ir diretamente adiante do ponto inicial em B.

<em>Dica de otimização:</em> quando fazemos [code]priceA = sum $ map snd pathA[/code], estamos calculando o preço do caminho em todo passo. Não teríamos de fazer isso se implementássemos [code]roadStep[/code] como uma função de [code](Path, Path, Int, Int) -&gt; Section -&gt; (Path, Path, Int, Int)[/code] onde os inteiros representam o melhor preço em A e em B.

Agora que temos uma função que leva um par de caminhos e uma seção e produz um novo caminho ideal, podemos apenas facilmente fazer uma dobra a esquerda sobre uma lista de seções. [code]roadStep[/code] é chamada com [code]([],[])[/code] e a primeira seção retorna um par de caminhos ideias para tal seção. Então, é chamada com esse par de caminhos e com a próxima seção e assim por diante. Quando tivermos passado por todas as seções, teremos um par com os caminhos ideais e o mais curto deles será a resposta. Com isso em mente, podemos implementar [code]optimalPath[/code].



Nós dobramos pela esquerda sobre [code]roadSystem[/code] (lembre-se, é uma lista de seções) com o acumulador inicial sendo um par de caminhos vazios. O resultado dessa dobra é um par de caminhos, então casamos padrões no par para conseguir os caminhos em si. Então, checamos qual desses é mais barato e retornamos ele. Antes de retorná-lo, tambem o invertemos, porque os caminhos ótimos até agora estavam invertidos devido a nossa escolha de prefixá-los em vez de anexá-los no final.
Vamos testar isso!



Esse é o resultado que deveríamos conseguir! Incrível! Ele difere um pouco do noso resultado esperado porque há um passo [code](C,0)[/code] no fim, que significa que nós cruzamos para a outra estrada assim que estamos em Londres, mas como esse cruzamento não custa nada, ainda é o resultado correto.
Temos a função que encontra um caminho ótimo, agora apenas temos que ler a representação textual do sistema de estradas da entrada padrão, convertê-la no tipo [code]RoadSystem[/code], rodar isso pela nossa função [code]optimalPath[/code] e imprimir o caminho.
Primeiramente, vamos fazer uma função que recebe uma lista e a divide em grupos do mesmo tamanho. Vamos chamá-la [code]groupsOf[/code]. Para o parâmetro de [code][1..10][/code], [code]groupsOf 3[/code] deve retornar [code][[1,2,3],[4,5,6],[7,8,9],[10]][/code].

Uma função recursiva padrão. Por um [code]xs[/code] de [code][1..10][/code] e um [code]n[/code] de [code]3[/code], isso é igual a [code][1,2,3] : groupsOf 3 [4,5,6,7,8,9,10][/code]. Quando a recursão estiver feita, temos nossa lista em grupos de três. Aqui está nossa função [code]main[/code], que lê da entrada padrão, cria um [code]RoadSystem[/code] a partir disso e imprime o caminho mais curto:


Primeiro, pegamos todo o conteúdo da entrada padrão. Então, chamamos [code]lines[/code] como nosso conteúdo para converter algo como [code]"50\n10\n30\n...[/code] para [code]["50","10","30"..[/code] e então nós mapeamos [code]read[/code] para converter isso em uma lista de números. Chamamos [code]groupsOf 3[/code] nela para que nós a tornemos uma lista de listas de tamanho 3. Mapeamos o lambda [code](\[a,b,c] -&gt; Section a b c)[/code] sobre essa lista de listas. Como você pode ver,  lambda apenas pega uma lista de tamanho 3 e transforma-a numa seção. Então [code]roadSystem[/code] é agora nosso sistema de estradas e tem até o tipo correto, a saber [code]RoadSystem[/code] (ou [code][Section][/code]). Chamamos [code]optimalPath[/code] com isso e então conseguimos o caminho e o preço em uma representação textual legal e a imprimimos.

Salvamos o texto a seguir


em um arquivo chamado [code]paths.txt[/code] e então o alimentamos para nosso programa.


Funciona como mágica! Você pode usar seu conhecimento do módulo [code]Data.Random[/code] para gerar um sistema de estradas muito maior, o qual você pode então usar para alimentar o que acabamos de escrever. Se você obtiver estouros de pilha, tente usar [code]foldl'[/code] em vez de [code]foldl[/code], porque [code]foldl'[/code] é estrito.