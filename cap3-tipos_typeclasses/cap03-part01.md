Tipos e Typeclasses
===================

** Acredite no tipo **

J� falamos que Haskell possui um sistema de tipos est�tico. O tipo de toda express�o � conhecido na hora da compila��o, o que resulta num c�digo mais seguro. Se voc� escrever um programa que tente dividir um tipo booleano por um n�mero, ele nem compilar�. Isso � bom porque � melhor detectarmos erros logo ao terminar de programar do que se deparar com travamentos indesejados. Tudo em Haskell tem um tipo, ent�o o compilador pode considerar v�rias possibilidades antes mesmo de compil�-lo.

Ao contr�rio de Java ou Pascal, Haskell tem infer�ncia de tipo. Se for digitado um n�mero, n�o precisamos avisar ao Haskell que � um n�mero. Ele consegue identificar isso automaticamente, n�o damos os tipos de fun��es e express�es explicitamente. N�s veremos apenas o b�sico de Haskell com uma vis�o apenas superficial sobre tipos. No entanto, entender o sistema de tipos � muito importante para o aprendizado de Haskell.

Um "tipo � algo como uma etiqueta que toda express�o t�m. Nos diz em qual categoria ela se encaixa. A express�o [code]True[/code] � booleana, [code]"hello"[/code] � uma String, etc.

Agora usaremos o GHCI para descobrir os tipos de algumas express�es. Faremos isso usando o comando [code]:t[/code] que, seguido de qualquer express�o v�lida, retorna o seu tipo. Vamos dar uma olhada.

Fazendo isso, vemos que [code]:t[/code] mostra a express�o seguida de [code]::[/code] e seu tipo. [code]::[/code] pode ser lido como "do tipo". Tipos expl�citos sempre s�o referidos pela sua primeira letra mai�scula. [code]'a'[/code], como � visto, � do tipo [code]Char[/code]. N�o � dif�cil de identificar que vem da palavra <i>caracter</i>. [code]True[/code] � [code]Boolean[/code]. Faz sentido. Mas, o que � isso? Examinando o tipo de [code]"HELLO!"[/code] descobrimos que ele � [code][Char][/code]. Os colchetes denotam uma lista. Ent�o lemos isso como uma <i>lista de caracteres</i>. Ao contr�rio de listas, cada valor da tupla tem seu tipo. Ent�o a express�o [code](True, 'a')[/code] tem o tipo [code](Bool, Char)[/code], e uma express�o como [code]('a','b','c')[/code] dever� retornar [code](Char, Char, Char)[/code]. [code]4 == 5[/code] sempre retornar� [code]False[/code], que � do tipo [code]Bool[/code].


Fun��es tamb�m t�m tipos. Quando escrevemos nossas pr�prias fun��es, podemos declarar explicitamente quais s�o os seus tipos. Isso geralmente � considerado uma boa pr�tica exceto quando a fun��o � muito curta. Daqui pra frente daremos v�rios exemplos que fazem uso de declara��o de tipos. Voc� ainda se lembra daquela compreens�o de lista que criamos no cap�tulo anterior e que usava filtros para retornar somente a parte mai�scula de uma string? Ent�o, aqui esta ela novamente com os tipos declarados.


[code]removeNonUppercase[/code] tem o tipo [code][Char] -&gt; [Char][/code], o que significa que ele parte de uma string e chega em outra string. Ou seja, ele ir� receber como par�metro uma string e nos devolver outra string como resultado. O tipo [code][Char][/code] � sin�nimo de [code]String[/code] ent�o ficaria mais claro se fosse escrito [code]removeNonUppercase :: String -&gt; String[/code]. N�o precisariamos fazer essa declara��o de tipo para o compilador porque ele poderia inferir por si s� que essa fun��o recebe uma string e retorna outra string. E se n�s precisarmos de uma fun��o que recebe v�rios tipos como par�metro? Vamos ver ent�o uma fun��o bem simples que pega tr�s inteiros e os soma:


Os par�metros s�o separados por [code]-&gt;[/code] e n�o h� nenhuma distin��o entre tipos de par�metros e retorno. O tipo do retorno � o �ltimo e os par�metros s�o os tr�s primeiros. Mais adiante veremos o porqu� deles serem apenas separados por um [code]-&gt;[/code] ao inv�s de ter um destaque maior como [code]Int, Int, Int -&gt; Int[/code] ou algo do g�nero.

Se voc� deseja especificar o tipo da fun��o, mas n�o tem certeza de qual deve ser, voc� pode escrev�-la normalmente e depois descobrir com o [code]:t[/code]. Fun��es tamb�m s�o express�es, ent�o [code]:t[/code] funciona sem problemas.

Aqui temos um apanhado geral dos principais tipos.

[type]Int[/code] � inteiro. � usado por n�meros inteiros. [code]7[/code] pode ser um [code]Int[/code] mas [code]7.2[/code] n�o. [code]Int[/code] possui limita��es de tamanho, o que significa ter um m�ximo e um m�nimo. Geralmente computadores 32bit t�m um [code]Int[/code] m�ximo de 2147483647 e um m�nimo de -2147483648.

[type]Integer[/code] significa, hmmm... inteiro tamb�m. A principal diferen�a � que n�o tem limita��es e pode ser usado por n�meros realmente grandes. Digo, extremamente grandes. Contudo, [code]Int[/code] � mais eficiente.


[type]Float[/code] � um n�mero real em ponto flutuante de precis�o simples.

[type]Double[/code] � um n�mero real em ponto flutuante com o dobro(!) de precis�o.

[type]Bool[/code] � um tipo booleano. Pode ter apenas dois valores: [code]True[/code] ou [code]False[/code].

[type]Char[/code] representa um caractere. � delimitado por aspas simples. Uma lista de caracteres � denominada String.

Tuplas s�o tipos mas possuem uma varia��o de acordo com a quantidade e valores que cont�m, ent�o teoricamente temos tuplas com infinitos tipos, o que � demais para cobrir nesse tutorial. Note que uma tupla vazia [code]()[/code] � tamb�m um tipo e pode assumir apenas um valor: [code]()[/code]