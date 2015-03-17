Maior do melhor
===============

A função [code]maximum[/code] recebe uma lista de coisas que podem ser ordenadas (por exemplo, 
instâncias da typeclass [code]Ord[/code]) e retorna a maior delas. Pense em como você implementaria 
isso de forma imperativa. Provavelmente você criaria uma variável para guardar o valor máximo encontrado 
até então e aí percorreria cada um dos elementos da lista. Caso um elemento fosse maior que o máximo, 
você o substituiria pelo anterior. O máximo que permanecesse ao fim seria o resultado. Ei! Foram bastante 
palavras para definir um algoritmo tão simples!

Agora vamos pensar em como definiríamos isso recursivamente. Poderíamos já dizer que o maior número de uma lista 
com um elemento é ele próprio. Então que o maior de uma lista com mais de um elemento é o primeiro 
elemento, caso este seja maior que o maior do resto. Se for o maior do resto, bem, então será ele. 
Isso! Agora vamos implementar em Haskell.

Como você pode ver, pattern matching e recursão formam uma grande dupla! A maioria das linguagens 
imperativas não tem pattern matching, o que leva você a criar muitas estruturas if/else para testar 
as condições limites. Aqui, nós simplesmente as colocamos como patterns. Então a primeira condição 
limite diz que se uma lista está vazia, <i>crash</i>! Faz sentido porque qual é o maior elemento de um lista 
vazia? Eu não sei. O segundo pattern também funciona como uma condição limite. Ele diz que se a lista 
tem apenas um único elemento, devemos apenas retorná-lo.

Agora, no terceiro pattern é onde a ação acontece. Nós usamos pattern matching para dividir a lista em 
primeiro elemento e resto. Usamos também uma associação <i>where</i> para definir [code]maxTail[/code] como 
o maior do resto da lista. Então nós testamos se o primeiro é maior que <i>maxTail</i>. Se for, 
nós retornamos ele. Se não, retornamos <i>maxTail</i>.

Peguemos o exemplo de uma lista de números e vejamos como isso funcionaria neles: [code][2,5,1][/code]. 
Se chamarmos [code]maximum'[/code] com ela, os primeiros dois patterns não vão ser encontrados. 
O terceiro irá e a lista então será dividida em [code]2[/code] e [code][5,1][/code]. A cláusula 
<i>where</i> tem o objetivo de descobrir o maior de [code][5,1][/code], e então nós seguimos por aí. 
Ela testa o terceiro pattern novamente e [code][5,1][/code] é dividido em [code]5[/code] e 
[code][1][/code]. Novamente, a cláusula [code]where[/code] quer saber o maior de [code][1][/code]. 
Como essa é a condição limite, ela retorna [code]1[/code]. Finalmente! Então voltando um grau de 
execução, comparamos [code]5[/code] com o maior de [code][1][/code] (que é [code]1[/code]) e 
chegamos a [code]5[/code]. Então agora já sabemos que o maior de [code][5,1][/code] é [code]5[/code]. 
Subimos novamente, onde tínhamos [code]2[/code] e [code][5,1][/code]. Comparando [code]2[/code] com o 
maior de [code][5,1][/code] ([code]5[/code]), nós escolhemos o [code]5[/code].

Uma forma ainda mais clara de escrever essa função seria com [code]max[/code]. Caso se lembre dela, 
[code]max[/code] é uma função que pega dois números e retorna o maior deles. Aqui está como nós 
poderíamos reescrever [code]maximum'[/code] usando [code]max[/code]:

Quanta sofisticação! Em essência, o maior elemento de uma lista é o maior entre o primeiro e o maior 
do resto da lista.
