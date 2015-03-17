Tipos sinônimos
===============

Anteriormente, mencionamos que quando escrevemos tipos, os tipos [code][Char][/code] e [code]String[/code] são equivalentes e substituíveis. Isso é implementado com <em>tipos sinônimos </em>. Tipos sinônimos na verdade não fazem nada por si só, eles são apenas um jeito de dar nomes diferentes para alguns tipos fazerem mais sentido quando alguém estiver lendo o nosso código e a documentação. Veja como a biblioteca padrão define [code]String[/code] como um sinônimo para [code][Char][/code].


Nós já fomos apresentados a palavra reservada <i>type</i>. A palavra reservada pode ser confusa para alguns, pois não estamos fazendo nada de novo (nós já fizemos isso com a palavra reservada <i>data</i>), mas apenas estamos criando um sinônimo para um tipo que já existente.

Se criarmos uma função que converte uma string para CAIXA ALTA e chamá-la de [code]toUpperString[/code] ou qualquer outro nome, podemos dar a ela uma declaração do tipo [code]toUpperString :: [Char] -&gt; [Char][/code] ou [code]toUpperString :: String -&gt; String[/code]. Ambas são essencialmente a mesma coisa, com a diferença de que a última é bem mais agradável de se ler.

Quando estávamos mexendo com o módulo [code]Data.Map[/code], nós primeiro representamos uma agenda como sendo uma lista de associações antes de converte-la em um mapeamento. Como já descobrimos antes, uma lista de associações é uma lista com pares de chaves-valores. Vamos dar uma olhada na nossa agenda (phoneBook).



Nós vimos que o tipo de [code]phoneBook[/code] é [code][(String,String)][/code]. Isso nos diz que ela é uma lista de associações que mapeia de uma string para outra string, só isso. Vamos criar um tipo sinônimo que transmita alguma informação a mais na declaração do tipo.


Agora a declaração de tipo para nossa agenda pode ser feita como [code]phoneBook :: PhoneBook[/code]. Vamos fazer também um tipo sinônimo para [code]String[/code].



Dar um tipo sinônimo a [code]String[/code] é algo que um programador Haskell faz quando ele quer transmitir mais informação sobre como a string pode ser usada em sua função e o que ela representa.

Então, quando implementarmos agora uma função que pega um nome e um número e soubermos que essa combinação de nome e número está em nossa agenda (PhoneBook), vamos poder dar uma declaração muito mais bonita e descritiva sobre o tipo.



Se nós decidirmos não usar tipos sinônimos, nossas funções podem ter um tipo como [code]String -&gt; String -&gt; [(String,String)] -&gt; Bool[/code]. Neste caso, a declaração de tipo que aproveita o uso de tipos sinônimos é mais fácil de entender. Entretanto, você não pode fazer tudo com eles. Nós introduzimos tipos sinônimos ou para descrever o que algum tipo existente representa em nossas funções (e assim nossas declarações de tipos se tornam mais bem documentadas) ou quando algum tipo é grande demais para ser repetido várias vezes (como [code][(String,String)][/code]) mas representa algo mais específico no contexto das nossas funções.

Tipos sinônimos podem ser parametrizados. Se nós quisermos que um tipo represente uma lista de associação de tipos mas permaneça genérico o suficiente para ser usado com qualquer tipo de chaves e valores, nós podemos fazer isso:



Agora, a função que obtêm o valor de uma chave na lista de associações pode ter o seguinte tipo [code](Eq k) =&gt; k -&gt; AssocList k v -&gt; Maybe v[/code]. O tipo [code]AssocList[/code] é um construtor que pega dois tipos e produz um tipo concreto, como [code]AssocList Int String[/code], por exemplo.

<em>Fonzie disse:</em> Aaay! Quando eu falo sobre <i>tipos concretos</i>, quero dizer, tipos totalmente aplicados como [code]Map Int String[/code] ou se nós estamos lidando com uma das funções polimórficas, [code][a][/code] ou [code](Ord a) =&gt; Maybe a[/code] e assim por diante. E assim também, as vezes eu e outros dizemos que [code]Maybe[/code] é um tipo, mas não quer dizer, porque todo idiota sabe que [code]Maybe[/code] é um construtor. Quando eu aplico um tipo extra a [code]Maybe[/code], como [code]Maybe String[/code], então eu tenho um tipo concreto. Você sabe, valores podem apenas ter tipos que são tipos concretos! Então como conclusão, viva intensamente, ame muito e não deixe ninguém mexer no seu favo de mel!

Assim como podemos aplicar parcialmente funções para pegar novas funções, nós podemos aplicar parcialmente tipos paramétricos e pegar novos construtores de tipo a partir deles. Assim como chamamos uma função com poucos parâmetros e pegamos uma nova função como retorno, nós podemos também especificar um construtor de tipos com poucos parâmetros e pegar de volta um construtor de tipo aplicado parcialmente. Se nós queremos um tipos que represente um mapeamento (de [code]Data.Map[/code]) de inteiros para alguma coisa, nós podemos então fazer isto:



Ou nós podemos fazer algo como isto:



De qualquer forma, o código do construtor de tipos [code]IntMap[/code] pega um parâmetro e esse será o tipo ao qual os inteiros irão apontar.

<em>Ah sim</em>. Se você tentar implementar isso, provavelmente você irá fazer uma importação qualificada de [code]Data.Map[/code]. Quando você faz uma importação qualificada, construtores de tipos também tem de ser precedidos com o nome do módulo. Então você escreveria [code]type IntMap = Map.Map Int[/code].

Tenha certeza de que você realmente entendeu a diferença entre construtores de tipos e construtores de valores. Só porque nós fizemos um tipo sinônimo chamado [code]IntMap[/code] ou [code]AssocList[/code] não significa que nós podemos fazer coisas como [code]AssocList [(1,2),(4,5),(7,9)][/code]. Tudo isso significa que nós podemos nos referir a esse tipo usando diferentes nomes. Nós podemos fazer [code][(1,2),(3,5),(8,9)] :: AssocList Int Int[/code], que irá fazer os números dentro dela assumir o tipo de [code]Int[/code], mas nós podemos ainda continuar usando essa lista como faríamos com qualquer lista normal que tem pares de inteiros dentro dela. Tipos sinônimos (e tipos em geral) somente podem ser usados na porção de tipos do Haskell. Nós estamos na porção de tipos do Haskell sempre quando definimos novos tipos (como na declaração de <i>data</i> e <i>type</i>) ou quando estamos após um [code]::[/code]. O [code]::[/code] esta em declarações de tipos ou em anotações de tipos.

Outro tipo interessante de dados que recebe dois tipos como parâmetros é o tipo [code]Either a b[/code]. É assim que ele é mais ou menos definido:



Há dois construtores de valores. Se [code]Left[/code] for usado, então o seu conteúdo é do tipo [code]a[/code] e se [code]Right[/code] for usado, então o seu conteúdo é do tipo [code]b[/code]. Podemos então usar esse tipo para encapsular o valor de um tipo no outro e então quando temos um valor do tipo [code]Either a b[/code], nós normalmente vamos verificar o padrão de correspondência entre [code]Left[/code] e [code]Right[/code] e diferenciar as coisas com base no que eles eram.



Até agora, nós vimos que [code]Maybe a[/code] foi usado principalmente para representar os resultados de computações que poderiam ter falhado ou não. Mas algumas vezes, [code]Maybe a[/code] não e bom o suficiente porque [code]Nothing[/code] não transmite informação boa o suficiente de que algo falhou. Isso é legal para as funções que podem falhar de forma única ou se nos apenas estamos interessados em como e porque ela falhou. Um [code]Data.Map[/code] falha ao pesquisar apenas se a chave que nos estávamos procurando não estava no mapa, então nós sabemos exatamente o que aconteceu. Entretanto, quando nós estamos interessados em como alguma função falhou e porque, nos comumente usamos o resultado do tipo [code]Either a b[/code], onde [code]a[/code] e alguma espécie de tipo que pode nos dizer algo sobre um possível erro e [code]b[/code] é o tipo de uma computação realizada com sucesso. Portanto, erros usam o construtor de valor [code]Left[/code] enquanto os resultados usam [code]Right[/code].

Um exemplo: uma escola de ensino médio tem armários para que os estudantes tenham algum lugar para pôr os seus pôsteres do Guns'n'Roses. Cada armário tem uma combinação de código. Quando um estudante quer um novo armário, ele irá falar para o supervisor dos armários o número do armário que ele quer e então o supervisor lhe dará um código. Entretanto, se alguém já estiver usando o armário, ele não poderá dizer o código do armário e terá que pegar um diferente. Nós iremos usar um mapa a partir de [code]Data.Map[/code] para representar os armários. Ele vai mapear a partir dos números dos armários para um par de armários que estão em uso ou não e os códigos dos armários.



Coisa simples. Nós introduzimos um novo tipo de dados para representar se um armário está ocupado ou livre e nós criamos um tipo sinônimo para o código do armário. Nós também criamos um tipo sinônimo para mapear pares de inteiros com o estado do armário e o seu código. E agora, vamos criar uma função que busca por um código no nosso mapa de armários. Vamos usar o tipo [code]Either String Code[/code] para representar o nosso resultado, porque a nossa busca pode falhar de duas formas &mdash; o armário pode estar ocupado, neste caso nós não podemos dizer o código ou o número do armário pode nunca ter existido. Se a busca falhar, nós iremos usar uma [code]String[/code] que nos dirá o que aconteceu.




Fizemos uma busca normal no mapa. Se nós recebemos um [code]Nothing[/code], vamos retornar um valor do tipo [code]Left String[/code], dizendo que o armário não existe. Se encontramos ele, então vamos fazer uma checagem adicional para ver se o armário já está ocupado. Se ele estiver, retornamos um [code]Left[/code] dizendo que ele já está ocupado. Se não estiver ocupado, retornamos um valor do tipo [code]Right Code[/code], onde nós damos ao estudante o código correto do armário. Atualmente isto é um [code]Right String[/code], mas nós introduzimos aquele tipo sinônimo apenas como uma documentação adicional dentro da declaração do tipo. Aqui está um exemplo de mapeamento:



Agora vamos tentar buscar alguns códigos de armários.



Nós podiamos ter usado um [code]Maybe a[/code] para representar o resultado, mas nós não iriamos saber o porque de não podermos receber o código. Mas agora, nós temos informação sobre a falha no nosso tipo de resultado.
