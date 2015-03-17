Molhando os nosso pés com Maybe
===============================

Agora que nós temos uma vaga ideia sobre o que são monads, vamos ver se podemos tornar essa ideia um pouco menos vaga.

Para a grande surpresa de ninguém, [code]Maybe[/code] é um monad, então vamos explora-lo um pouco mais e ver se podemos combina-lo com o que nós aprendemos sobre monads.

Tenha certeza que você entendeu <a href="functors-applicative-functors-and-monoids#applicative-functors">applicatives</a> até aqui. Será bom se você já tiver uma noção de como as várias instâncias de [code]Applicative[/code] trabalham e que tipo de cálculo elas representam, porque monads não são nada mais do que retomar o nosso conhecimento prévio e aprimorar ele.

Um valor do tipo [code]Maybe a[/code] representa um valor do tipo [code]a[/code] com o contexto de uma possível falha anexada. Um valor de [code]Just "dharma"[/code] significa que a string [code]"dharma"[/code] existe enquanto que o valor de [code]Nothing[/code] representa sua ausência, ou se você olhar para a string como um resultado de um cálculo, isso significa que o cálculo falhou.

Quando nós olhamos para [code]Maybe[/code] como uma functor, vimos que se iterarmos uma função [code]fmap[/code] sobre ela, ela será mapeada internamente se o valor for [code]Just[/code], caso contrário o [code]Nothing[/code] será mantido porque não há nada para mapear.

Assim:

Assim como um applicative functor, que funciona de forma semelhante. Contudo, applicatives também tem a função de embalar as coisas. [code]Maybe[/code] é um applicative functor de tal forma que, quando usamos [code]&lt;*&gt;[/code] para aplicar uma função dentro de um [code]Maybe[/code] para um valor que está dentro de um [code]Maybe[/code], ambos tem que ser [code]Just[/code] para o resultado ser um valor [code]Just[/code], caso contrário o resultado é [code]Nothing[/code]. Faz sentido porque se você tiver perdido a função ou a coisa que você está aplicando, você não poderá fazer nada lá fora com o ar rarefeito, então você terá que propagar uma falha:

Quando usamos o estilo applicative para ter funções normais agindo em valores [code]Maybe[/code] isto é semelhante. Todos os valores têm que ser [code]Just[/code], caso contrário todos são [code]Nothing[/code]!

E agora vamos pensar como faríamos [code]&gt;&gt;=[/code] para [code]Maybe[/code]. Como nós falamos, [code]&gt;&gt;=[/code] tem um valor monadico, e uma função que tem um valor normal e retorna um valor monadico e consegue aplicar essa função para um valor monadico. Como ele faz isso, se a função tem um valor normal? Bem, para fazer isso, ele tem que levar em conta o contexto desse valor monadico.

Neste caso, [code]&gt;&gt;=[/code] levaria um valor [code]Maybe a[/code] e uma função do tipo [code]a -&gt; Maybe b[/code] que de alguma maneira aplica a função para o [code]Maybe a[/code]. Para descobrir como ele faz isso, podemos usar a intuição que temos de [code]Maybe[/code] ser um applicative functor. Vamos dizer que temos uma função [code]\x -&gt; Just (x+1)[/code]. Ela pega um número, acrescenta [code]1[/code] a ele e o embala com um [code]Just[/code]:

Se alimentarmos ela com [code]1[/code], ela será calculada como [code]Just 2[/code]. Se nós dermos a ela o número [code]100[/code], o resultado será [code]Just 101[/code]. Muito simples. Agora aqui vai o chute: Como nós alimentamos um valor [code]Maybe[/code] para esta função? Se nós pensarmos sobre como [code]Maybe[/code] atua como um applicative functor, responder isso será muito fácil. Se alimentarmos isso com um valor [code]Just[/code], pegarmos o que está dentro de [code]Just[/code] e aplicarmos a função nele. Se der a ele um [code]Nothing[/code], hmm, bem, então estaremos com uma função porém [code]Nothing[/code] (nada) para aplicar nela. Neste caso vamos apenas fazer o que nós fizemos antes de dizer que o resultado é [code]Nothing[/code].

Ao invés de chamar isso de [code]&gt;&gt;=[/code], iremos chama-lo de [code]applyMaybe[/code] por enquanto. Ela irá pegar um [code]Maybe a[/code] e uma função que retorna um [code]Maybe b[/code] e manipula-los para aplicar esta função ao [code]Maybe a[/code]. Aqui está o código:

Ok, agora vamos jogar com ele um pouco. Vamos usa-lo como uma função infixa com o valor de [code]Maybe[/code] no lado esquerdo e a função no lado direto:

No exemplo acima, nós vimos que quando usamos [code]applyMaybe[/code] com um valor [code]Just[/code] e uma função, a função simplesmente será aplicada no valor dentro de [code]Just[/code]. Quando tentarmos usar isso com um [code]Nothing[/code], o resultado completo será [code]Nothing[/code]. Que tal se a função retornar apenas um [code]Nothing[/code]? Vamos ver:

Exatamente o que nós esperávamos. Se o valor monadico à esquerda é um [code]Nothing[/code], a coisa toda é [code]Nothing[/code]. E se a função a direita retorna um [code]Nothing[/code], o resultado novamente é [code]Nothing[/code]. Isto é muito similar quando nós usamos [code]Maybe[/code] como um applicative e nós obtemos um resultado [code]Nothing[/code] se em alguma parte havia um [code]Nothing[/code].

Parece que para [code]Maybe[/code], nós temos que descobrir como tiramos um valor imaginário e alimentamos ele com uma função que recebe um valor normal e retorna um imaginário. Nós fazemos isso mantendo em mente que um valor [code]Maybe[/code] representa um cálculo que pode ter falhado.

Você pode estar se perguntado, como é que isso pode ser útil? Pode parecer que applicative functors são mais fortes que monads, quando applicative functors nos permitem ter uma função normal para faze-la funcionar operando em valores com contextos. Vamos ver que monads podem fazer isso também, porque eles são uma evolução de applicative functors, e que eles também podem fazer alguma coisas legais que applicative functors não podem.

Iremos voltar para [code]Maybe[/code] em um minuto, mas primeiro, vamos verificar que tipo de classe pertence a monads.