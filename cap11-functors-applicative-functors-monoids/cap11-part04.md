A palavra chave newtype
=======================

Até agora, nós aprendemos como criar tipos de dados algébricos utilizando a palavra chave <em>data</em>. Nós aprendemos também a dar sinônimos para tipos existentes com a palavra chave <em>type</em>. Nesta seção, nós iremos dar uma olhada em como criar novos tipos a partir de tipos de dados já existentes utilizando a palavra chave <em>newtype</em>, e porque nós iriamos querer fazer isso em primeiro lugar.

Na seção anterior, nós vimos que há na verdade mais formas do tipo lista ser um applicative functor. Uma delas é [code]&lt;*&gt;[/code] pegar cada função da lista a esquerda e aplicar a cada valor na lista a direita, resultando em todas as combinações possíveis ao aplicar as funções da esquerda com os valores da direita.

A segunda forma é pegar a primeira função a esquerda do [code]&lt;*&gt;[/code] e aplicar ao primeiro valor da direita, então pegar a segunda função da lista a esquerda e aplicar ao segundo valor da direita, e assim por diante. Por fim, é como se estivessemos mesclando as duas listas. Mas listas já são uma instância de [code]Applicative[/code], então como nós fazemos da lista uma instância de [code]Applicative[/code] desta segunda forma? Se você lembra, nós dissemos que o tipo [code]ZipList a[/code] foi introduzido para esse propósito, que tem um construtor de valor, [code]ZipList[/code], que tem apenas um campo. Nós envolvemos a lista no campo. Então, [code]ZipList[/code] se torna uma instância de [code]Applicative[/code], assim quando nós queremos usar listas como aplicatives para realizar mesclagem, nós apenas envolvemos elas com o construtor de valor [code]ZipList[/code] e então uma vez feito, recuperamos elas com [code]getZipList[/code]:

Então, o que isso tem a ver com a palavra-chave <i>newtype</i>? Bem, penso sobre como nós podemos escrever a declaração dos dados para nosso tipo [code]ZipList a[/code]. Uma forma seria fazer assim:

Um tipo que tem apenas um construtor e que o construtor de valor tem apenas um campo que é uma lista de coisas. Nós podemos querer também usar a sintaxe do record onde nós automaticamente obtemos uma função que extrai uma lista de um [code]ZipList[/code]:

Parece bom e funciona muito bem. Nós temos duas forma de fazer um tipo existente uma instância de um tipo de classe, então nós usamos a palavra-chave <i>data</i> para envolver o tipo em outro e fazer desse uma instância desse segunda modo.

A palavra-chave <i>newtype</i> em Haskell é exatamente para estes casos quando nós queremos apenas pegar um tipo e envolver em alguma coisa para aprensentar como outro tipo. Nas bibliotecas atuais, [code]ZipList a[/code] é definido como algo assim:

Ao invés da palavra-chave <i>data</i>, a palavra-chave <i>newtype</i> é usada. Por que motivo? Bem, <i>newtype</i> é rápido. Se você usa a palavra-chave <i>data</i> para envolver um tipo, há um custo ao envolver e recuperar os dados quando seu programa está sendo executado. Mas se você usa <i>newtype</i>, Haskell sabe que você apenas está usando isso para envolver um tipo existente em um novo tipo (portanto o nome), porque você quer que internamente seja a mesma coisa mas tenha um tipo diferente. Com isso em mente, Haskell consegue eliminar o trabalho de envolver e recuperar valores uma vez que este sabe que valor é de que tipo.

Então, porque não usar <i>newtype</i> em todos os casos ao invés de <i>data</i>? Bem, quando você cria um novo tipo a partir de um tipo existente usando a palavra-chave <i>newtype</i>, você pode ter apenas um construtor e este deve ter apenas um campo. Mas com <i>data</i>, você pode criar tipos de dados com mais de um construtor de valor e cada construtor de valor pode ter zero ou mais campos:

Quando usamos <i>newtype</i>, estamos restritos apenas um construtor com apenas um campo.

Nós também podemos usar a palavra-chave <i>deriving</i> com <i>newtype</i> assim como usamos com <i>data</i>. Nós podemos derivar instâncias para [code]Eq[/code], [code]Ord[/code], [code]Enum[/code], [code]Bounded[/code], [code]Show[/code] e [code]Read[/code]. Se derivarmos a instância para um tipo de classe, o tipo que estamos envolvendo tem de estar nesses tipos também. Faz sentido, porque <i>newtype</i> apenas envolve um tipo existente. Então agora, se nós fizermos o seguinte, nós podemos imprimir e comparar valores do nosso tipo:

Vamos dar uma olhada:

Nesse <i>newtype</i> em particular, o valor do construtor de valor tem o seguinte tipo:

Este recebe um valor do tipo [code][Char][/code], como [code]"my sharona"[/code] e retorna um valor do tipo [code]CharList[/code]. A partir dos exemplos acima onde nós usamos o construtor [code]CharList[/code], nós vimos que esse é realmente um caso. Inversamente, a função [code]getCharList[/code], o qual foi gerada para nós já que usamos a sintaxe record em nosso newtype, tem este tipo:

Este recebe um valor do tipo [code]CharList[/code] e converte para um valor do tipo [code][Char][/code]. Você pode pensar nisso como envolvendo e recuperando, mas você também pode pensar nisso como uma conversão de valores de um tipo para outro.

<h3>Usando newtype para criar instâncias de um tipo de classe</h3>

Muitas vezes, nós queremos fazer dos nossos tipos instâncias de um certo tipo de classe, mas o tipo dos parâmetros não casam com o que nós queremos fazer. É fácil fazer de [code]Maybe[/code] uma instância de [code]Functor[/code], porque o tipo de classe [code]Functor[/code] é definido assim:

Então nós apenas começamos com:

E então implementamos [code]fmap[/code]. Todos os tipos de parâmetros fazem sentido aqui porque [code]Maybe[/code] toma o lugar de [code]f[/code] na definição do tipo de classe [code]Functor[/code], então se nós olharmos [code]fmap[/code] como se ele apenas funcionasse com [code]Maybe[/code], ele acaba se comportando da seguinte maneira:

Não é excelente? Agora, e se nós quisermos fazer da tupla uma instância de [code]Functor[/code], de uma forma que quando nós usarmos [code]fmap[/code] em uma tupla este é aplicado ao primeiro item da tupla? Dessa forma, fazendo [code]fmap (+3) (1,1)[/code] resultaria em [code](4,1)[/code]. Escrever uma instância para isso parece difícil. Com [code]Maybe[/code] nós apenas fazemos [code]instance Functor Maybe where[/code], porque apenas construtores de valores de tipo que recebem exatamente um parâmetro podem se tornar uma instância de [code]Functor[/code]. Mas parece que não tem uma forma de fazer algo assim com [code](a,b)[/code], então o parâmetro do tipo [code]a[/code] acaba sendo o parâmetro que muda quando nós usamos [code]fmap[/code]. Para contornar isso, nós podemos usar <i>newtype</i> em nossa tupla de uma forma que o segundo parâmetro represente o tipo do primeiro item na tupla:

E agora, nós podemos fazer disso uma instância de [code]Functor[/code] da forma que a função é mapeada sobre o primeiro componente:

Como você pode ver, nós podemos casar padrões de tipos definidos com <i>newtype</i>. Nós casamos padrões para obter a tupla subjacente, então nós aplicamos a função [code]f[/code] ao primeiro componente da tupla e então usamos o construtor [code]Pair[/code] para converter a tupla de volta para [code]Pair b a[/code]. Se nós imaginarmos que o tipo [code]fmap[/code] poderia ser se apenas trabalhasse nos nossos novos pares, este seria:

Novamente, escrevemos [code]instance Functor (Pair c) where[/code] e assim [code]Pair c[/code] toma o lugar de [code]f[/code] na definição do tipo de classe para [code]Functor[/code]:

Então agora, se nós convertermos a tupla em [code]Pair b a[/code], nós podemos usar [code]fmap[/code] nela e a função será mapeada sobre o primeiro componente:


<h3>A avaliação sob demanda de newtype</h3>

Nós mencionamos que <i>newtype</i> geralmente é mais rápido que <i>data</i>. A única coisa que pode ser feita com <i>newtype</i> é transformar um tipo existente em um novo tipo, então internamente, Haskell pode representar os valores dos tipos definidos com <i>newtype</i> como os originais, apenas tem de manter em mente que seus tipos agora são distintos. Isso significa que <i>newtype</i> não é apenas mais rápido, este é também avaliado sob demanda. Vamos dar uma olhada no que isso significa.

Como dissemos antes, Haskell é avaliado sob demanda por padrão, o que significa que apenas quando nós tentarmos exibir os resultados de nossas funções as computações serão feitas. Além disso, apenas as computações que são necessárias para nossa função exibir o resultado serão avaliadas. O valor [code]undefined[/code] em Haskell representa uma computação errônea. Se tertarmos avaliar isso (ou seja, forçar Haskell a realizar esta computação) exibindo o resultado no terminal, Haskell irá ter um chilique (conhecido técnicamente como exceção):

Entretanto, se nós criarmos uma lista com alguns valores [code]undefined[/code] e apenas usarmos o topo da lista, que não é [code]undefined[/code], tudo vai funcionar porque Haskell de fato não precisará avaliar nenhum outro elemento na lista se nós apenas queremos ver o que o primeiro elemento é:

Agora considere o seguinte tipo:

Este é o seu tipo de dado algébrico comum que foi definido usando a palavra chave <i>data</i>. Ele tem apenas um construtor, com um único campo que o tipo é [code]Bool[/code]. Vamos criar uma função para fazer um casamento de padrões em [code]CoolBool[/code] e retornar o valor [code]"hello"[/code], independente se [code]Bool[/code] dentro de [code]CoolBool[/code] for [code]True[/code] ou [code]False[/code]:

Ao invés de aplicar esta função a um [code]CoolBool[/code] normalmente, vamos fazer diferente e aplicar a [code]undefined[/code]!

Caramba! Uma exceção! Agora, porque esta exceção acontece? Tipos definidos com a palavra chave <i>data</i> podem ter múltiplos construtores de valores (mesmo [code]CoolBool[/code] tendo apenas um). Então para ver se o valor passado para nossa função obedece ao padrão [code](CoolBool _)[/code], Haskell tem de avaliar o tipo apenas para ver que construtor de valor foi usado quando nós criamos o valor. E quando nós tentarmos avaliar um valor [code]undefined[/code], uma exceção será lançada.

Ao invés de usar a palavra chave <i>data</i> para [code]CoolBool[/code], vamos tentar usar <i>newtype</i>: 

Nós não temos de mudar nossa função [code]helloMe[/code], porque a sintaxe do casamento de padrões é a mesma se você usar <i>newtype</i> ou <i>data</i> para definir nosso tipo. Vamos fazer a mesma coisa aqui e aplicar [code]helloMe[/code] a um valor [code]undefined[/code]:

Agora funcionou! Hmmm, por quê? Bem, como dissemos antes, quando nós usamos <i>newtype</i>, Haskell pode internamente representar os valores de novos tipos da mesma forma que os valores originais. Ele não precisa adicionar uma caixa em torno deles, apenas precisa estar ciente de que os valores são de tipos diferentes. E porque Haskell sabe que os tipos criados com a palavra chave <i>newtype</i> podem ter apenas um construtor, ele não tem de avaliar o valor passado para a função para ter certeza que obedece aos padrões de [code](CoolBool _)[/code] porque os tipos criados com <i>newtype</i> podem ter apenas um construtor de valor possível e um campo!

Esta diferença de comportamento pode parecer trivial, mas é muito importante porque isso nos ajuda a perceber que mesmo tipos definidos com <i>data</i> e <i>newtype</i> se comportam de maneira similar do ponto de vista de programadores porque ambos tem construtores e campos, eles são apenas dois mecanismos diferentes. Enquanto que <i>data</i> pode ser usado para criar seus próprios tipos, <i>newtype</i> é para criar um novo tipo baseado em um tipo existente. Casar padrões em valores <i>newtype</i> não é como extrair algo de uma caixa (como é com <i>data</i>), é mais como fazer uma conversão direta entre um tipo e outro.

<h3>[code]type[/code] vs. [code]newtype[/code] vs. [code]data[/code]</h3>

Até este ponto, você pode estar um pouco confuso sobre qual exatamente é a diferença entre <i>type</i>, <i>data</i> e <i>newtype</i>, então vamos refrescar nossas memórias um pouco.

A palavra chave <em>type</em> é para criar tipos sinônimos. O que significa é que nós podemos apenas dar outro nome para um tipo que já existe apenas para facilitar quando precisarmos referenciá-lo. Vamos dizer que fizemos o seguinte:

O que tudo isso faz é nos permitir referenciar o tipo [code][Int][/code] como [code]IntList[/code]. Eles podem ser usados em conjunto. Nós não temos um construtor de valor [code]IntList[/code] ou nada parecido. Porque [code][Int][/code] e [code]IntList[/code] são apenas duas formas de referenciar o mesmo tipo, não importa qual nome nós usamos em nossas anotações de tipos:

Nós usamos tipos sinônimos quando nós queremos fazer nossas assinaturas de tipos mais descritivas, dando aos tipos nomes que nos dizem algo sobre seu propósito no contexto das funções onde estão sendo usadas. Por exemplo, quando nós usamos uma lista de associação do tipo [code][(String,String)][/code] para representar uma agenda telefônica, nós demos a este tipo o sinônimo de [code]PhoneBook[/code], para que as assinaturas de nossas funções ficassem fáceis de ler.

A palavra chave <em>newtype</em> é para pegar tipos existentes e envolver eles em novos tipos, principalmente para facilitar fazer deles instâncias de certos tipos de classes. Quando nós usamos <i>newtype</i> para envolver um tipo existente, o tipo que nós obtemos é diferente do tipo original. Se criarmos o seguinte <i>newtype</i>:

Nós não podemos usar [code]++[/code] para juntar um [code]CharList[/code] e uma lista do tipo [code][Char][/code]. Nós não podemos nem mesmo usar [code]++[/code] para juntar duas [code]CharList[/code], porque [code]++[/code] trabalha apenas em listas e o tipo[code]CharList[/code] não é uma lista, mesmo esta contendo uma. Nós podemos, entretando, converter duas [code]CharList[/code] para listas, juntar elas com [code]++[/code] e então converter de volta para um [code]CharList[/code].

Quando nós usamos a sintaxe record em nossas declarações <i>newtype</i>, nós obtemos funções para converter entre o novo tipo e o tipo original: nomeando o construtor de valor de nosso <i>newtype</i> e a função para extrair o valor do seu campo. O novo tipo também não se torna automaticamente uma instância dos tipos de classes que o tipo original pertence, então nós temos que derivar ou manualmente escrever elas.

Na prática, você pode pensar em declarações <i>newtype</i> como declarações <i>data</i> que possuem apenas um construtor e um campo. Se você se pega escrevendo declarações <i>data</i> assim, considere usar <i>newtype</i>.

A palavra chave <em>data</em> é para criar seus próprios tipos e com eles, fazer o que você quiser. Eles podem ter quantos construtores e campos você desejar e podem ser usados para implementar qualquer tipo de dados algébricos que quiser. Qualquer coisa como listas e [code]Maybe[/code] - como tipos de árvores.

Se você apenas quer que suas assinaturas de tipos pareçam claras e mais descritivas, você vai querer tipos sinônimos provavelmente. Se você quer pegar um tipo existente e envolver em um novo tipo para fazer deste uma instância de um tipo de classe, pode ser que esteje precisando de <i>newtype</i>. E se você quer fazer algo completamente novo, a probabilidade é de que você use a palavra chave <i>data</i>.
