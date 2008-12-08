= Trabalhando com Array

Assim como em outras linguagens de programação, array no ruby é indexado por inteiros e começa com o número 0(zero).

Assim como em PHP, por exemplo, array no ruby é dinâmico, ou seja, não é necessário informar qual o tamanho, apesar de ser permitido. Mesmo que o tamanho inicial seja informado na sua criação, ele pode aumentar, sem que seja feita quaisquer alteração no código da aplicação.

Diferente de quando trabalhamos com coleções, em Java, é possível armazenar diversos tipos de objetos em um único array em ruby. Aliás, não se armazena o objeto propriamente dito, e sim um ponteiro, uma referência para acessá-lo. A menos que o objeto seja imediato, por exemplo, um objeto da classe Fixnum.

== Criando e inicializando um array

Em ruby, existem diversas maneiras de se criar/inicializar um

[source, ruby]
------------------------
a1 = ["a","b","c","d","e"]
a2 = Array.[]("a","b","c","d","e")
a3 = Array["a","b","c","d","e"]
a4 = Array.new
a5 = Array.new(5)
a6 = Array.new(3,"OLA MUNDO")
a7 = %w{a b c d e}
------------------------
	
Em ruby, existem diversas maneiras de se criar/inicializar um array. Vejamos algumas:

Tudo parece legal e auto-explicativo, mas algumas pessoas estranham a última expressão. Temos 3 formas de se criar um array quando utilizamos o método new:

1º - Sem parâmetro
2º - Com 1 parâmetro (tamanho inicial do array a ser criado).
3º - Com 2 parâmetros. O primeiro parâmetro refere-se ao tamanho inicial a ser criado e o segundo parâmetro ao valor inicial de cada posição.

Se você foi esperto o suficiente, deve ter notado que eu falei que objetos não são armazenados diretamente em um array, o que existe são ponteiros para o mesmo objeto, com exeção de valores imediatos, como objetos da classe Fixnum. Então, o que você acha que acontece quando indicamos um valor inicial para cada posição no array, e, logo em seguida alteramos o valor de uma posição?

Acertou se você falou que todas as posições serão trocadas. Como seja definido um valor default no momento da criação do array, ruby cria apenas um objeto e faz com que todas as posições referenciem o mesmo objeto. Veja o código abaixo, que ilustra isso:

[source, ruby]
------------------------
a6 = Array.new(3,"OLA MUNDO") #=> ["OLA MUNDO","OLA MUNDO","OLA MUNDO"]
a6[0].downcase! # [" ola mundo"," ola mundo"," ola mundo"]
------------------------

Mas, então, qual o objetivo de se criar um array de N posições e iniciá-las com um valor default? Calma! Existe uma forma de contornar esse comportamente, digamos, estranho. Basta usar um bloco, e cada posição referenciará um objeto diferente. Quando utilizamos um bloco, estamos dizendo ao ruby para criar um objeto para cada posição

[source, ruby]
------------------------
a6 = Array.new(3) { "OLA MUNDO" } #=> ["OLA MUNDO","OLA MUNDO","OLA MUNDO"]
a6[0].downcase! #=> ["ola mundo","OLA MUNDO","OLA MUNDO"]
------------------------
Quando um array cresce e um novo elemento é adicionado, o valor default é nil:

[source, ruby]
------------------------
array = Array.new
array[0] = "Alberto"
array[5] = "Leal"	#=> ["Alberto", nil,nil,nil,nil, "Leal"]
------------------------

== Acessando e Atribuindo Elementos

Para acessar algum elemento em um array, basta informar o índice ao método [ ]:

[source, ruby]
------------------------
array = [1,2,3,4,5,6,7,8,9]
primeiro_elemento = array[0]
terceiro_elemento = array[2]
------------------------

Existe, ainda, uma outra opção, basta informar o índice ao método at:

[source, ruby]
------------------------
array = [1,2,3,4,5,6,7,8,9]
primeiro_elemento = array.at(0)
terceiro_elemento = array.at(2)
------------------------

Para ambos os métodos, [] e at, temos a opção de informar um inteiro negativo. Dessa forma, a contagem de posições para acessar o elemento será feita da direita para a esquerda. Por exemplo:

[source, ruby]
------------------------
array = [1,2,3,4,5,6,7,8,9]
primeiro_elemento = array[-2]
terceiro_elemento = array.at(-3)
------------------------

Se desejar verificar uma quantidade específica de posições, basta informar a quantidade desejada como segundo parâmetro para o método []. E, como primeiro parâmetro, informe a partir de qual posição no array que será iniciado. Exemplo:

[source, ruby]
------------------------
array = [1,2,3,4,5,6,7,8,9]
primeiro_elemento = array[0..2]
------------------------

O método slice é um atalho para o método [], portanto:

[source, ruby]
------------------------
primeiro_elemento = array[-2]
------------------------
é o mesmo que:

[source, ruby]
------------------------
primeiro_elemento = array.slice(-2)
------------------------
Caso você queira recuperar várias posições, existe o método values_at:

[source, ruby]
------------------------
array = [1,2,3,4,5,6,7,8,9]
result = array.values_at(0,2,4) #=> 1,3,5
result2 = array.values_at(0..2,4) #=> 1,2,3,5
------------------------
== Tamanho de um Array

Dois métodos são bastante usados quando é desejável obter o tamanho de um array.

	* length - possui um alias size
	* nitems - desconsidera posições nulas (nil)

	array = [1,2,3,4,5,6,7,8,9, nil, 10]
	puts array.length #=> 11
	puts array.size #=> 11
	puts array.nitems #=> 10
	
* Se quiser visualizar o conteúdo de um array, utilize o método inspect.

== Comparando arrays

Para comparar arrays, utilizamos o método <=>. Esse método retorna -1 quando o objeto do lado esquerdo da operação é menor do que o lado direito, 0 quando os objetos são iguais e 1 quando o objeto do lado direito é maior do que o do lado esquerdo.

Quando comparamos arrays, os elementos são comparados um a um, posição a posição. Assim que a primeira comparação falha, isto é, quando os objetos não são iguais, a comparação chega ao fim.Vejamos um exemplo:

	b1 = [1,2,3,4,5]
	b2 = [1,2,4,1,1]
	re = b1 <=> b2 #=> -1
	
Quando é feita a comparação na posição 2, é constatado que b1 é menor do que b2, mesmo que todo o resto de b2 seja menor do que b1.

Quando os elementos dos arrays são iguais, então a comparação retorna 0. Por outro lado, se os arrays possuem tamanhos diferentes, mas seus elementos são iguais até o momento da desigualdade de tamanho, o array que possuir um tamanho maior é considerado maior.

	c1 = [1,2,3,4,5]
	c2 = [1,2,3,4,5,6,7]
	re = c1 <=> c2  # -1
	
Falei anteriormente que, é permitido armazenar diferente tipos de objetos dentro de um mesmo array. Então, o que aconteceria se tentarmos comparar arrays, onde elementos de diferentes tipos tivessem de ser comparados?

Nesse caso, haverá um erro, ArgummentError!

	d1 = [1,2,3,4,5]
	d2 = [1,2,”nb”,4,5]
	if d1 > d2
		print "não aconteceu o erro"
	end
	ArgumentError: comparison of Array with Array failed
		from (irb):60:in `>'
		from (irb):60

Entretando, se for comparado arrays de diferentes tamanhos, e o "elemento estranho" estiver em uma posição superior, ou seja, que não será necessário fazer a comparação, não há nenhum problema

	d1 = [1,2,3,4,5]
	d2 = [1,2,3,4,5,"nb"]
	if d1 > d2
		print "não aconteceu o erro"
	end
		
