# Trabalhando com Strings
	
## Sendo uma string

String é basicamente uma sequência de caracteres. Essencialmente, existem duas formas de se criar uma objeto string em ruby: utilizando aspas simples ou aspas duplas. Existe uma sutil diferença entre essas duas abordagens.

Quando criamos uma string utilizando aspas simples estamos forçando que ela seja impresa exatamente da forma como foi escrita. E, se for necessário incluir uma ou mais aspas simples no texto, é preciso escapar (\).

Por outro lado, se criar uma string utilizando aspas duplas é permitido utilizar de um artifício, que veremos mais a frente, chamado interpolação. Além de poder usufruir de outros caracteres de escape, como: \n,\t e outros.

	s1 = "Isso é uma string"
	s2 = 'Isso, também, é uma string'
	s3 = %q[Mais uma forma de se criar uma string] # 'Mais uma forma de se criar uma string'
	s4 = %Q[Outra forma de se criar uma string] # "Outra forma de se criar uma string"

* Nota: Repare que quando utilizamos %q, aspas simples é utilizada, enquanto que %Q utiliza aspas duplas

## Substituição de strings

Para alterar trechos de uma string, utiliza-se os métodos gsub e sub. 
O método sub altera apenas a primeira ocorrência encontrada para o padrão informado para substituição, por exemplo:

	str = "prazer ruby e ruby on rails"
	str.sub(/ruby/, "xxx") #"prazer xxx e ruby on rails"

Agora, já o método gsub faz a substituição em todas as ocorrências encontradas:

	str = "prazer ruby e ruby on rails"
	str.sub(/ruby/, "xxx") #"prazer xxx e xxx on rails"

## Pesquisando Strings

Para retirar uma substring de uma string, basta informar o índice e a quantidade de caracteres que deseja obter a partir daquele índice:

	str = "Prazer, Ruby on Rails"
	str[8,4] # “Ruby”

Juntamente com o código acima, ou melhor, antes de utilizá-lo, pode ser interessante pesquisar de ante-mão qual a posição da palavra “Ruby”, para então recuperá-la. Caso não se saiba qual é a posição em que a palavra se encontra, pode-se fazer uma chamada ao método index ou rindex:

	str.index(/ruby/) #nil

O código acima não retorna nada, visto que a palavra procurada não existe dentro da string. O que quer dizer que nossa busca está procurando apenas pela palavra em caixa baixa. Portanto, para torná-la não case-sensitive, adicionamos a letra “i” ao final de nossa expressão regular:

	str.index(/ruby/i) #8

Existe, ainda, o método scan. Utiliza-se tal método passando um padrão a ser pesquisado, via regex. O retorno é um array com o resultado encontrado:

	str.scan(/\w/) # ["P", "r", "a", "z", "e", "r", "R", "u", "b", "y", "o", "n", "R", "a", "i", "l", "s"]
	str.scan(/\w+/) # ["Prazer", "Ruby", "on", "Rails"]

## Interpolação

Ao construir uma string, muitas vezes desejamos adicionar valores de variáveis ao longo da expressão. Para não ter que ficar concatenando sempre que isso for necessário, utiliza-se da interpolação. A notação utilizada para tal tarefa é #{}. Basta adicionarmos o nome da nossa variável dentro da notação para que ela seja impressa. Mas, para isso, a string deve ser construída com aspas duplas, caso contrário, tanto a notação quanto o nome da variável, contida dentro da mesma, serão exibidos literalmente.

	minha_variavel = “Rails”
	str = "Prazer, Ruby on #{minha_variavel}" # “Prazer, Ruby on Rails”
	str = 'Prazer, Ruby on #{minha_variavel}' # ‘Prazer, Ruby on #{minha_variavel}’ 

## Alguns métodos interessantes

length ou size - Retorna o tamanho da string.

	str = “Prazer, Ruby”
	puts str.length
	puts str.size

each_byte - utilizado para percorrer byte a byte.
	
	str = “Prazer, Ruby”
	str.each_byte { |c| puts c }	

split - retorna um array de acorda com o critério passado.

	str = “Prazer, Ruby”
	str.split #[“Prazer,”, “Ruby”]
	str.split(“,”) #[“Prazer”, “Ruby”]

ljust, rjust, center - Formatar uma string

	str = “Prazer, Ruby”
	str.ljust(20) # "Prazer, Ruby        "
	str.rjust(20) # "        Prazer, Ruby"
	str.center(20) # "    Prazer, Ruby    "

upcase 			- Deixar todas as letras maiúsculas
downcase 		- Deixar todas as letras minúsculas
capitalize 	- Deixar a primeira letra em maiúscula
swapcase 		- Inverte as letras, ou seja, as maiúsculas ficam minúsculas e vice-versa

	str = “Prazer, Ruby”
	str.downcase
	str.upcase
	str.capitalize
	str.swapcase

strip - utilizado para remover espaços no início e no final da string
ltrip - utilizado para remover espaços no início da string
rtrip - utilizado para remover espaços no final da string
	
	str = “    Prazer, Ruby    ”
	str.strip # "Prazer, Ruby on Rails"
	str.ltrip # "Prazer, Ruby on Rails    "
	str.rtrip # "   Prazer, Ruby on Rails"

squeeze - Remove caracteres duplicados (Apenas se os caracteres estiverem “juntos”)

	str = “Prazer, RRuby”
	str.squeeze # "Prazer, Ruby"

## Invertendo uma String

Para inverter uma string, utilize o método reverse:
	
	str = “Prazer, Ruby”
	str.reverse # "ybuR ,rezarP"

Se, ao invés de inverter as letras, você desejar inverter as palavras:

	str = "Prazer, Ruby"
	str.split(“ ”).reverse.join(“ ”) # "Ruby Prazer,"

O que acontece no exemplo acima é que o que está sendo invertido é a ordem dos elementos contidos no array :-)

## Convertendo String em Inteiro

Existem duas formas de se converter uma string em um inteiro. Ou você utiliza os métodos da classe kernel (Interger e/ou Float) ou os métodos da classe String (to_i e/ou to_f).

Mas, vale lembrar que, para que os métodos funcionem o valor da variável deve representar um valor numérico. Caso contrário, o valor retornado será 0(zero) ou erro:

	num = “um”
	num = Interger(num) # ArgumentError
	num = num.to_i # 0
	num = num.to_f # 0.0

* Nota: Se o valor da variável começar com um número, mesmo o restante sendo string, os métodos to_i e to_f convertem os números iniciais:

	num = “123um”
	num = Interger(num) # ArgumentError
	num = num.to_i # 123
	num = num.to_f # 123

## Tamanho de String

Para contar a quantidade de caracteres em uma string, utilize o método size :

	num = “Tamanho da String”
	puts num.size # 17

Você pode, também, contar a ocorrência de determinada palavra ou expressão:

	num = “Tamanho da String”
	puts num.count(“a”) # 3
