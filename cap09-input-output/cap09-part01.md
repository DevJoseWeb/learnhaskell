Entrada e Saída
===============

Nós mencionamos que Haskell é uma linguagem puramente funcional. Enquanto nas linguagens imperativas você usualmente faz as coisas passando ao computador uma série de passos para executar, em programação funcional define-se mais como as coisas são. Em Haskell, uma função não pode mudar algum estado, como mudar o conteúdo de uma variável (quando uma função muda um estado, dizemos que a função tem <i>efeitos colaterais</i>). A única coisa que uma função pode fazer em Haskell é nos devolver um resultado baseado nos parâmetros que passamos a ela. Se uma função é chamada duas vezes com os mesmos parâmetros, ela tem que devolver o mesmo resultado. Embora possa parecer um pouco limitador quando você vem de um mundo imperativo, temos visto que isso é na verdade muito legal. Em uma linguagem imperativa, você não tem garantia que uma simples função que deveria apenas moer alguns números não vai incendiar sua casa, sequestrar seu cachorro e arranhar seu carro com uma batata enquanto mói esses números. Por exemplo, quando estamos fazendo uma árvore binária de busca, nós não inserimos um elemento em uma árvore modificando alguma árvore existente. Nossa função ao inserir em uma árvore binária de busca na verdade retorna uma nova árvore, porque nós não podemos modificar a original.

Embora funções incapazes de mudar estado sejam boas porque nos ajudam a manter o foco no nosso programa, existe um problema com elas. Se uma função não pode mudar qualquer coisa no mundo, como ela poderá nos dizer o que ela calculou? Com intúito de nos dizer o que ela calculou, ela tem que mudar o estado de um dispositivo de saída (usualmente o estado da tela), a qual então emite fótons que viajam para o nosso cérebro e mudam o estado da nossa mente.

Não se desespere, nem tudo está perdido. Percebe-se que Haskell na verdade tem um sistema bastante hábil para lidar com funções que têm efeitos colaterais que nitidamente separa a parte pura do nosso programa da parte impura, a qual faz todo o trabalho sujo de se comunicar com o teclado e com a tela. Com essas duas partes separadas, nós ainda podemos nos concentrar na parte pura do nosso programa e levar vantagem de todas as coisas que a pureza oferece, como preguiça, robustez e modularidade enquanto nos comunicamos eficientemente com o mundo externo.

** Olá, mundo! **

Até agora, nós sempre carregamos nossas funções no GHCI para testá-las e brincar com elas. Nós também exploramos a biblioteca padrão de funções dessa maneira. Mas agora, após oito capítulos ou mais, nós finalmente vamos escrever nosso primeiro programa Haskell <i>real</i>! E com bastante certeza, vamos usar o bom e velho artifício do [code]"olá, mundo"[/code].

<em>Ei!</em> No decorrer desse capítulo, irei assumir que você está utilizando um ambiente baseado em unix para aprender Haskell. Se você esta no Windows, eu sugiro que você faça o download do <a href="http://www.cygwin.com/">Cygwin</a>, que é um ambiente baseado em Linux para Windows, também conhecido como "exatamente o que você procura".

Então, para os iniciantes, digite o seguinte no seu editor de texto favorito:

Nós acabamos de definir um nome chamado [code]main[/code] e nele nós chamamos uma função [code]putStrLn[/code] com o parâmetro [code]"hello, world"[/code]. Parece medíocre, mas não é, como nós veremos dentro de poucos momentos. Salve esse arquivo como [code]helloworld.hs[/code].

E agora, nos vamos fazer algo que nunca fizemos antes. Vamos finalmente compilar nosso programa! Estou muito animado! Abra o seu terminal e navegue até o diretório onde está o arquivo [code]helloworld.hs[/code] e faça o seguinte:

Ok! Com alguma sorte, você verá algo como isto e agora você poderá executar seu programa através do comando [code]./helloworld[/code].

E aqui estamos nós, nosso primeiro programa compilado que imprimiu alguma coisa no terminal. Que extraordinariamente chato!

Vamos analizar o que nós escrevemos. Primeiro, vamos olhar para o tipo da função [code]putStrLn[/code].

Nós podemos ler o tipo de [code]putStrLn[/code] assim: [code]putStrLn[/code] pega uma string e retorna uma <em>ação de I/O</em> que tem um resultado do tipo [code]()[/code] (ou seja: uma tupla vazia, também conhecida como unidade). Uma ação de I/O é alguma coisa que, quando executada, irá realizar uma ação com um efeito colateral (que é usualmente ler do dispositivo de entrada ou imprimir algo na tela) e irá também conter algum tipo de valor de retorno dentro dela. Imprimir uma string no terminal realmente não tem nenhum tipo de valor de retorno significativo, então um valor irrelevante [code]()[/code] é usado.

A tupla vazia é um valor [code]()[/code] e também tem um tipo [code]()[/code].</div>

<p>Então, quando será executada uma ação de I/O? Bem, será onde [code]main[/code] estiver. Uma ação de I/O será executada quando nós dermos a ela um nome [code]main[/code] e então executarmos nosso programa.

Seu programa ser somente uma ação de I/O parece um tipo de limitação. Por isso nós podemos usar a sintaxe <i>do</i> para unir várias ações de I/O em uma só. Veja o seguinte exemplo:

Ah, interessante, nova sintaxe! E isso parece muito mais com um programa imperativo. Se você compilar e tentar executar, ele irá se comportar exatamente como você esperaria. Note que nós escrevemos <i>do</i> e então escrevemos uma série de passos, como faríamos em um programa imperativo. Cada um desses passos é uma ação de I/O. Colocando-as juntas com a sintaxe <i>do</i>, nós as transformamos em uma única ação de I/O. A ação que nós obtivemos tem um tipo [code]IO ()[/code], porque esse é o tipo da última ação de I/O dentro dela.

Por causa disso, [code]main[/code] sempre tem uma assinatura de tipo [code]main :: IO <i>alguma coisa</i>[/code], em que [code]<i>alguma coisa</i>[/code] é algum tipo concreto. Por convenção, nós usualmente não especificamos uma declaração de tipo para [code]main[/code].

Uma coisa interessante que nós não tínhamos visto antes é a terceira linha, que diz [code]name &lt;- getLine[/code]. Parece que uma linha é lida do dispositivo de entrada e armazenada em uma variável chamada [code]name[/code]. É realmente isso? Bem, vamos examinar o tipo de [code]getLine[/code].

Aha, ok. [code]getLine[/code] é uma ação de I/O que contém um resultado do tipo [code]String[/code]. Isso faz sentido, porque ela irá aguardar que o usuário digite algo no terminal e então que algo seja representado como uma string. Então o que acontece com [code]name &lt;- getLine[/code]? Você pode ler esse trecho de codigo assim: <em>execute a ação de I/O [code]getLine[/code] e então associe o valor do seu resultado a [code]name[/code]</em>. [code]getLine[/code] tem um tipo [code]IO String[/code], então [code]name[/code] terá um tipo [code]String[/code]. Você pode pensar em uma ação de I/O como sendo uma caixa com pequenos pés que irá sair para o mundo real e fazer algo lá (como grafitar algo em uma parede) e talvez trazer consigo alguma informação. Uma vez que ela tenha trazido as informações a você, a única maneira de abrir a caixa e obter a informação dentro dela é usando o operador [code]&lt;-[/code]. E se nós estamos tirando dados de uma ação de I/O, só podemos fazê-lo quando estamos dentro de outra ação de I/O. É dessa forma que Haskell nitidamente separa as partes pura e impura do nosso código. Desse ponto de vista [code]getLine[/code] é impura porque não há garantia de que o seu resultado será o mesmo quando executada duas vezes. Isso ocorre devido a uma espécie de <i>contaminação</i> pelo uso do construtor de tipo [code]IO[/code] e nós somente podemos retirar esses dados da ação de I/O dentro de código de I/O. Além disso, devido ao código de I/O também ser contaminado, qualquer computação que dependa de informação de I/O contaminada terá um resultado contaminado.

Dê uma olhada nesse trecho de código. Ele é válido?

Se você disse não, vá comer um biscoito. Se disse sim, beba um copo de lava derretida. Brincadeira, não beba! A razão pela qual isso não funciona é que [code]++[/code] requer que ambos os parâmetros sejam listas do mesmo tipo. O parâmetro da esquerda tem o tipo [code]String[/code] (ou [code][Char][/code] se quiser), enquanto [code]getLine[/code] tem tipo [code]IO String[/code]. Você não pode concatenar uma string e uma ação de I/O. Primeiro temos que tirar o resultado de dentro da ação de I/O para obter um valor do tipo [code]String[/code] e a única maneira de fazer isso é usar algo como [code]name &lt;- getLine[/code] dentro de alguma outra ação de I/O. Se nós queremos lidar com informação impura, temos que fazê-lo em um ambiente impuro. Então a contaminação da impureza se espalha em volta quase como a praga dos zumbis e é do nosso maior interesse manter os trechos de I/O do nosso código o menor possível.

Toda ação de I/O executada tem um resultado encapsulado dentro dela. Por isso nosso exemplo anterior poderia ter sido escrito assim:

Contudo, [code]foo[/code] teria apenas um valor [code]()[/code], então fazer isso seria um pouco questionável. Observe que nós não associamos o resultado do último [code]putStrLn[/code] a nada. Isso porque em um bloco <i>do</i>, <em>o resultado da última ação não pode ser associado a um nome</em> como o das duas primeiras foram. Veremos exatamente por que isto acontece um pouco mais tarde, quando nos aventurarmos pelo mundo dos monads. Por enquanto, você pode pensar nisso como se um bloco <i>do</i> extraisse automaticamente o resultado da última ação e o associasse ao seu próprio resultado.

Exceto pela última linha, todas as linhas em um bloco <i>do</i> que não têm seus resultados associados a nenhum nome podem ser escritas como uma associação. Então [code]putStrLn "BLAH"[/code] pode ser escrito como [code]_ &lt;- putStrLn "BLAH"[/code]. Mas isso é inútil, então nós omitimos o [code]&lt;-[/code] para ações de I/O que não contêm um resultado importante, como [code]putStrLn <i>alguma coisa</i>[/code].

Iniciantes algumas vezes pensam que escrevendo

irá ler do dispositivo de entrada e então associar o valor a [code]name[/code]. Bem, não irá, tudo que isso faz é dar à ação de I/O [code]getLine[/code] um nome diferente chamado [code]name[/code]. Lembre-se, para retirar o valor de uma ação de I/O, você tem que executá-la dentro de outra ação de I/O associando seu resultado a um nome com [code]&lt;-[/code].

Ações de I/O serão executadas somente se for dado a elas um nome [code]main[/code] ou quando elas estiverem dentro de uma ação de I/O maior que nós fizermos com um bloco <i>do</i>. Nós podemos também usar um bloco <i>do</i> para unir algumas ações de I/O e então podemos usar a ação de I/O resultante em outro bloco <i>do</i> e assim por diante. De qualquer forma, elas serão executadas somente se eventualmente cairem em [code]main[/code].

Ah, certo, existe ainda mais um caso em que ações de I/O serão executadas. Quando nós digitamos uma ação de I/O no GHCI e pressionamos enter, ela é executada.

Mesmo quando nós digitamos um número ou a invocação de uma função no GHCI e pressionamos enter, ele irá avaliar (se for necessário) e invocar [code]show[/code] para o resultado da avaliação e então irá imprimir essa string no terminal usando [code]putStrLn[/code] implicitamente.

Lembra das associações <i>let</i>? Se não se lembra, refresque sua memória lendo <a href="syntax-in-functions#let-it-be">essa seção</a>. Elas são construções da forma [code]let <i>associações</i> in <i>expressão</i>[/code], em que [code]<i>associações</i>[/code] são nomes que serão utilizados em uma expressão e [code]<i>expressão</i>[/code] é a expressão a ser avaliada que utilizará esses nomes. Também dissemos que em list comprehensions, a parte <i>in</i> não é necessária. Bem, você pode usá-las em blocos <i>do</i> quase da mesma forma que você as usa em list comprehensions. Dê uma olhada nisso:

Viu como as ações de I/O no bloco <i>do</i> estão alinhadas? Percebeu também como o <i>let</i> está alinhado com as ações de I/O e os nomes do <i>let</i> estão alinhados uns com os outros? Esta é uma boa prática, porque indentação é importante em Haskell. Agora, nós escrevemos [code]map toUpper firstName[/code], o que transforma algo como [code]"John"[/code] em uma string mais legal, como [code]"JOHN"[/code]. Nós associamos strings em maiúsculas a um nome e depois as utilizamos em outra string que imprimimos no terminal.

Você pode estar se perguntando quando usar [code]&lt;-[/code] e quando usar associações <i>let</i>? Bem, lembre-se, [code]&lt;-[/code] é usado (por enquanto) para executar ações de I/O e associar seus resultados a nomes. [code]map toUpper firstName[/code], contudo, não é uma ação de I/O. É uma expressão pura em Haskell. Então, use [code]&lt;-[/code] quando você quiser associar resultados de ações de I/O a nomes e associações <i>let</i> para associar expressões puras a nomes. Escrevendo algo como [code]let firstName = getLine[/code], só teríamos dado a ação de I/O [code]getLine[/code] um nome diferente e ainda teríamos que utilizar [code]&lt;-[/code] para executá-la.

Agora iremos fazer um programa que continuamente lê uma linha e imprime a mesma linha com as palavras invertidas. A execução do programa irá terminar quando nós digitarmos uma linha em branco. Este é o programa:

Para se ter uma noção do que isso faz, você pode executá-lo antes de analizarmos o código.

<em>Dica de profissional</em>: Para rodar um programa você pode compilá-lo e rodar o arquivo executável escrevendo [code]ghc --make helloworld[/code] e então [code]./helloworld[/code] ou você pode usar o comando [code]runhaskell[/code] dessa forma: [code]runhaskell helloworld.hs[/code] e o seu programa será executado dinamicamente.

Primeiro, vamos dar uma olhada na função [code]reverseWords[/code]. Ela é apenas uma função normal que pega uma string como [code]"hey there man"[/code] e então invoca [code]words[/code] com ela para gerar uma lista de palavras como [code]["hey","there","man"][/code]. Então mapeamos a lista com [code]reverse[/code], obtendo [code]["yeh","ereht","nam"][/code], juntamos as palavras de volta em uma string usando [code]unwords[/code] e o resultado final é [code]"yeh ereht nam"[/code]. Veja que usamos composição de funções aqui. Sem composição de funções, teríamos que escrever algo como [code]reverseWords st = unwords (map reverse (words st))[/code].

E quanto ao [code]main[/code]? Primeiro, obtemos uma linha digitada no terminal usando [code]getLine[/code] e chamamos essa linha de [code]line[/code]. E agora, temos uma expressão condicional. Lembre-se que em Haskell, todo <i>if</i> deve ter um <i>else</i> porque toda expressão tem que ter algum tipo de valor de retorno. Escrevemos um <i>if</i> de forma que quando a condição for verdadeira (em nosso caso, a linha que digitamos está em branco), executamos uma ação de I/O e quando não for, a ação de I/O no <i>else</i> é executada. Isto porque em um bloco <i>do</i> de I/O, <i>if</i>s tem que ser da forma [code]if <i>condição</i> then <i>ação de I/O</i> else <i>ação de I/O</i>.

Primeiro vamos dar uma olhada no que acontece no bloco <i>else</i>. Porque temos que ter exatamente uma ação de I/O depois do <i>else</i>, nós usamos um bloco <i>do</i> para unir duas ações de I/O em uma só. Você também poderia escrever esse trecho como:

Isso deixa mais explícito que o bloco <i>do</i> pode ser visto como uma ação de I/O, mas é mais feio. De qualquer forma, dentro do bloco <i>do</i>, nós chamamos [code]reverseWords[/code] passando como parâmetro a linha que obtivemos com [code]getLine[/code] e então imprimimos o resultado no terminal. Depois disso, nós executamos [code]main[/code]. Ela é chamada recursivamente sem problemas, porque a própria [code]main[/code] é uma ação de I/O. De certo modo, nós voltamos para o início do programa.

Agora, o que acontece quando [code]null line[/code] é verdadeiro? Nesse caso, o que está depois do <i>then</i> é executado. Se procurarmos, veremos que isso é [code]then return ()[/code]. Se você usa linguagens imperativas como C, Java ou Python, você provavelmente está pensando que sabe o que esse [code]return[/code] faz e talvez você já tenha pulado esse parágrafo realmente longo. Bem, essa é a questão: o <em>[code]return[/code] em Haskell não tem nada a ver com [code]return[/code] na maioria das outras linguagens!</em>. Ele tem o mesmo nome, o que confunde muitas pessoas, mas na verdade ele é um pouco diferente. Em linguagens imperativas, [code]return[/code] normalmente encerra a execução de um método ou subrotina e faz ele devolver algum tipo de valor a quem o chamou. Em Haskell (especificamente em ações de I/O), ele cria uma ação de I/O com um valor puro. Se você pensar na analogia feita antes sobre a caixa, ele pega o valor e coloca dentro de uma caixa. A ação de I/O resultante na verdade não faz qualquer coisa, ela apenas tem aquele valor encapsulado como seu resultado. Então, em um contexto de I/O, [code]return "haha"[/code] terá um tipo [code]IO String[/code]. Qual é o sentido de colocar um valor puro dentro de uma ação de I/O que faz coisa nenhuma? Por que contaminar nosso programa com mais [code]I/O[/code] do que o necessário? Bem, precisamos de alguma ação de I/O para executar no caso de uma entrada em branco. Por isso nós simplesmente forjamos uma ação de I/O que faz nada, escrevendo [code]return ()[/code].

Usar [code]return[/code] não faz com que o bloco de I/O <i>do</i> encerre sua execução ou algo parecido. Por exemplo, esse programa irá muito felizmente executar cada linha até a última:

Tudo o que esses [code]return[/code]s fazem é criar ações de I/O que na verdade fazem nada além de ter um resultado encapsulado e esses resultados são descartados porque eles não são associados a um nome. Podemos usar [code]return[/code] combinado com [code]&lt;-[/code] para associar coisas a nomes.

Como você pode ver, [code]return[/code] é como um de oposto [code]&lt;-[/code]. Enquanto [code]return[/code] pega um valor e o coloca em uma caixa, [code]&lt;-[/code] pega uma caixa (e a executa) e retira o valor de dentro dela, associando a um nome. Mas isso é redundante, especialmente porque você pode usar associações <i>let</i> em blocos <i>do</i> para associar valores a nomes, dessa forma:

Quando estamos lidando com blocos <b><i>do</i></b> de I/O, nós usamos [code]return[/code] principalmente porque precisamos criar uma ação de I/O que faz coisa nenhuma porque não queremos que essa ação de I/O criada a partir de um bloco <b><i>do</i></b> tenha o valor resultante da sua última ação, mas queremos que ela tenha um valor resultante diferente, então usamos [code]return[/code] para criar uma ação de I/O que sempre encapsula o valor desejado por nós e a colocamos no final.

Um bloco <b><i>do</i></b> também pode ter somente uma ação de I/O. Nesse caso, isso é o mesmo que simplesmente escrever a ação de I/O. Algumas pessoas prefeririam escrever [code]then do return ()[/code] nesse caso porque o <i>else</i> também tem um <i>do</i>.

Antes de passarmos aos arquivos, vamos dar uma olhada em algumas funções que são úteis quando lidamos com I/O.

[function]putStr[/function] é bem parecida com [code]putStrLn[/code] porque ela recebe uma string como parâmetro e retorna uma ação de I/O que imprimirá essa string no terminal, porém [code]putStr[/code] não quebra a linha depois de imprimir a string enquanto [code]putStrLn[/code] quebra.

Sua assinatura de tipo é [code]putStr :: String -&gt; IO ()[/code], então o resultado encapsulado na ação de I/O resultante é a tupla vazia. Um valor forjado, então não faz sentido associá-lo a um nome.

[function]putChar[/function] pega um caractere e retorna uma ação de I/O que o imprimirá no terminal.

[code]putStr[/code] na verdade é definida recursivamente utilizando [code]putChar[/code]. A condição de parada de [code]putStr[/code] é a string vazia, então se estamos imprimindo uma string vazia, somente retornamos uma ação de I/O que não faz qualquer coisa usando [code]return ()[/code]. Se a string não é vazia, então imprimimos o seu primeiro caractere usando [code]putChar[/code] e então imprimimos o restante usando [code]putStr[/code].

Observe como podemos usar recursão em I/O, exatamente como podemos usar em código puro. Exatamente como em código puro, definimos a condição de parada e então pensamos no que de fato é o resultado. Essa é uma ação que primeiro imprime o primeiro caractere e então imprime o restante da string.

[function]print[/function] pega um valor de qualquer tipo que seja uma instância de [code]Show[/code] (o que quer dizer que sabemos como representá-lo como uma string), invoca [code]show[/code] com esse valor para convertê-lo em string e então imprime a saída no terminal. Basicamente, ela é apenas [code]putStrLn . show[/code]. Primeiro ela roda [code]show[/code] em um valor e então passa o resultado para [code]putStrLn[/code], que retorna uma ação de I/O que imprimirá nossa entrada.

Como você pode ver, é uma função bem útil. Lembra que nós falamos que ações de I/O são executadas somente quando elas aparecem em [code]main[/code] ou quando tentamos avaliá-las no prompt do GHCI? Quando digitamos um valor (como [code]3[/code] ou [code][1,2,3][/code]) e pressionamos enter, o GHCI na verdade usa [code]print[/code] nesse valor para mostrá-lo no terminal!

Quando queremos imprimir strings, normalmente usamos [code]putStrLn[/code] porque não queremos as aspas circundando-as, mas para imprimir valores de outros tipos no terminal, [code]print[/code] é mais usada.

[function]getChar[/function] é uma ação de I/O que lê um caractere do dispositivo de entrada. Sendo assim, sua assinatura de tipo é [code]getChar :: IO Char[/code], porque o resultado contido na ação de I/O é um [code]Char[/code]. Observe que graças à bufferização, a leitura de caracteres na verdade não acontecerá até que o usuário pressione enter.

Esse programa aparentemente deveria ler um caractere e então checar se é um espaço em branco. Se é, interrompe a execução e se não, imprime-o no terminal e então faz a mesma coisa novamente. Bem, de certa forma ele faz isso, mas não do modo que você esperaria. Dê uma olhada nisso:

A segunda linha é a entrada. Nós digitamos [code]hello sir[/code] e pressionamos enter. Graças à bufferização, a execução do programa irá iniciar somente depois que tivermos pressionado enter e não depois que qualquer caractere for digitado. Uma vez que tivermos pressionado enter, ele atuará sobre tudo o que tivermos digitado até então. Experimente brincar com esse programa para ter uma noção sobre isso!

A função [function]when[/function] faz parte de [code]Control.Monad[/code] (então, escreva [code]import Control.Monad[/code] para acessá-la). Ela é interessante porque em um bloco <i>do</i> ela se parece com uma declaração de fluxo de controle, mas na verdade é uma função comum. Ela pega um valor booleano e uma ação de I/O , se o valor booleano for [code]True[/code], retorna a mesma ação de I/O que nós passamos. Todavia, se for [code]False[/code], ela retorna a ação [code]return ()[/code], que é uma ação de I/O que faz nada. Eis um modo como podemos reescrever o trecho de código anterior, onde demonstramos [code]getChar[/code], usando [code]when[/code]:

Então, como você pode perceber, ela é útil para encapsular o padrão [code]if <i>alguma coisa</i> then do <i>alguma ação de I/O</i> else return ()[/code].

[function]sequence[/function] pega uma lista de ações de I/O e retorna uma ação de I/O que irá executar as ações passadas uma após a outra. O resultado contido nessa ação de I/O será uma lista dos resultados de todas as ações de I/O que foram executadas. Sua assinatura de tipo é [code]sequence :: [IO a] -&gt; IO [a][/code]. Escrever isso:

É exatamente o mesmo que fazer isso:

Então [code]sequence [getLine, getLine, getLine][/code] cria uma ação de I/O que irá executar [code]getLine[/code] três vezes. Se associarmos essa ação a um nome, o resultado é uma lista de todos os resultados, no nosso caso, uma lista de três coisas que o usuário digitou no terminal.

Um padrão comum usando [code]sequence[/code] é quando usamos funções como [code]print[/code] ou [code]putStrLn[/code] para mapear listas. Escrevendo [code]map print [1,2,3,4][/code] não iremos criar uma ação de I/O. Isso irá criar uma lista de ações de I/O, porque isso é como escrever [code][print 1, print 2, print 3, print 4][/code]. Se quisermos transformar essa lista de ações de I/O em uma única ação de I/O, temos que sequenciá-la.

O que é esse [code][(),(),(),(),()][/code] no final? Então, quando avaliamos uma ação de I/O no GHCI, ela é executada e o seu resultado é impresso, a menos que o resultado seja [code]()[/code], caso em que nada é impresso. Daí porque avaliar [code]putStrLn "hehe"[/code] no GHCI só imprime [code]hehe[/code] (porque o resultado contido em [code]putStrLn "hehe"[/code] é [code]()[/code]). Mas quando escrevemos [code]getLine[/code] no GHCI, o resultado dessa ação de I/O é impresso, porque [code]getLine[/code] tem o tipo [code]IO String[/code].

Devido ao uso de uma função que retorne uma ação de I/O para mapear uma lista e depois sequenciá-la ser algo tão comum, as funções [function]mapM[/function] e [function]mapM_[/function] foram introduzidas. [code]mapM[/code] pega uma função e uma lista, mapeia a lista com a função e então sequencia o mapeamento. [code]mapM_[/code] faz o mesmo, mas descarta o resultado no final. Geralmente usamos [code]mapM_[/code] quando não nos importamos com os resultados que nossas ações de I/O sequenciadas têm.

[function]forever[/function] pega uma ação de I/O e retorna uma ação de I/O que somente repete para sempre a ação de I/O passada. Ela faz parte de [code]Control.Monad[/code]. Esse pequeno programa irá aguardar indefinidamente alguma entrada do usuário e mandar de volta a ele, EM MAIÚSCULAS:

[function]forM[/function] (localizada em [code]Control.Monad[/code]) é parecida com [code]mapM[/code], porém tem os parâmetros invertidos. O primeiro parâmetro é a lista e o segundo é a função que será usada para mapear a lista, que será então sequenciada. Por que isto é útil? Bem, com algum uso criativo de lambdas e notação <i>do</i>, podemos fazer coisas como essa:

O [code](\a -&gt; do ... )[/code] é uma função que pega um número e retorna uma ação de I/O. Temos que envolvê-la com parênteses, senão o lamba irá pensar que as últimas duas ações de I/O pertencem a ele. Observe que escrevemos [code]return color[/code] no bloco <i>do</i> mais interno. Fizemos isso para que a ação de I/O definida pelo bloco <i>do</i> tenha nossas cores contidas nele como resultado. Na verdade não temos que fazer isso, porque [code]getLine[/code] já as contém como resultado. Escrever [code]color &lt;- getLine[/code] e depois [code]return color[/code] é somente desempacotar o resultado de [code]getLine[/code] e em seguida empacotá-lo novamente, então é o mesmo que escrever somente [code]getLine[/code]. O [code]forM[/code] (chamado com seus dois parâmetros) produz uma ação de I/O, cujo resultado nós associamos a [code]colors[/code]. [code]colors[/code] é apenas uma lista de strings comum. No final, imprimimos todas essas cores escrevendo [code]mapM putStrLn colors[/code].

Você pode pensar que [code]forM[/code] é: faça uma ação de I/O para cada elemento nessa lista. O que cada ação de I/O fará pode depender do elemento que é usado para criar a ação. Finalmente, execute essas ações e associe seus resultados a alguma coisa. Nós não temos que associá-las, também podemos simplemente descartá-las.

Poderíamos ter feito isso sem [code]forM[/code], porém com [code]forM[/code] fica mais legível. Geralmente escrevemos [code]forM[/code] quando queremos mapear e sequenciar algumas ações que definimos lá no local utilizando a notação <i>do</i>. Seguindo esse raciocínio, poderíamos ter substituido a última linha por [code]forM colors putStrLn[/code].

Nesta seção, aprendemos o básico sobre <b><i>input</i></b> e <b><i>output</i></b> (I/O) em Haskell. Também descobrimos o que são ações de I/O, como elas nos habilitam a realizar operações de <i>input</i> e <i>output</i> e quando elas são realmente executadas. Reiterando, ações de I/O são valores muito parecidos com qualquer outro valor em Haskell. Podemos passá-los como parâmetros para funções e funções podem retornar ações de I/O como resultado. A diferença é que se elas estiverem na função [code]main[/code] (ou forem o resultado de uma linha no GHCI), elas são executadas. O que significa que elas escrevem coisas na sua tela ou tocam [a href="https://www.youtube.com/watch?v=ZnHmskwqCCQ"]Yakety Sax[/a] através das suas caixas de som. Cada ação de I/O também pode encapsular um resultado com o qual ela lhe diz o que ela obteve do mundo real.

Não pense em uma função como a [code]putStrLn[/code] como sendo uma função que pega uma string e a imprime na tela. Pense nela como uma função que pega uma string e retorna uma ação de I/O. Essa ação de I/O irá, quando executada, imprimir poesias bonitas no seu terminal.
