---
title: "Introdução rápida ao R-lang"
seoTitle: "Quick Guide to R Programming"
seoDescription: "Aprenda sobre instalação, tipos de dados, funções, operadores e mais no R com este guia introdutório detalhado"
datePublished: Thu Aug 25 2022 02:03:15 GMT+0000 (Coordinated Universal Time)
cuid: cl78ehsqg02xg36nv9ywf02z1
slug: introducao-rapida-ao-r-lang
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/oyXis2kALVg/upload/v1661393093689/QMMzjWP2x.jpeg
tags: r-language, statistics, data-science, r, learning, learn-coding, rlang

---

# Instalação

Aqui fiz um artigo sobre a instalação do R no Linux: [Menos de 3 minutos e estará pronto](https://esli.blog.br/instalando-o-r)

# R console/CLI

Basta digitar `R`

Para sair do console do R, use o `quit()`, irá perguntar se deseja salvar `Save workspace image? [y/n/c]:`

# Print

Declarando o valor `10` para a variavel `a` e depois imprimindo no output:

```c
a <- 10 #também podemos usar `a = 10`, mas depois explico!
print(a)

[1] 10
```

# Dados

O R classifica todos os dados com base em 5 tipos diferentes:

1. Double
    
2. Integer
    
3. Character
    
4. Logical
    
5. Complex
    

## Double

O Double é como o float no python, qualquer número inteiro ou decimal é um Double e ele é o modo default para qualquer número armazenado (no caso do `print(a)` acima, por exemplo)

## Integer

O integer, inteiro. por definição matemática literal, inteiro é um número inteiro (sem frações ou pontos decimais). Todos os inteiros são indicados por "L" após o número.

No nosso primeiro exemplo (lembre-se que é um Double por default!), vamos usa-lo como inteiro:

```c
 a <- 10L
print(a)

[1] 10
```

Show?! Complicando um pouco:

```c
 x=17.53L
Warning message:
integer literal 17.53L contains decimal; using numeric value 

 print(x)
[1] 17.53
```

O que houve?

`x` é uma fração, mas estamos tentando armazená-la como um Integer adicionando um L após 17.53. R entende e converte para Double para evitar perda de dados.

Se armazenarmos um valor de fração dentro de um Integer, o R armazenará automaticamente o valor como um Double para evitar qualquer perda de dados.

## Character

São nossas strings. Para usa-los, basta armazenar entre aspas. O R diferencia maiúscula e minúscula.

## Logical

Logical são os famosos booleanos, True (T) ou False (F)

## Complex

Complex são números que possuem uma combinação de números reais + imaginários. Ex.: 3+3i

# Declarando variáveis

Usa-se os operadores ( `->`, `<-` e `=` ) para declarar as variaveis.

```c
> alias = 1753
> alias <- 1753
> 1753 -> alias
> print(alias)
[1] 1753

> teste=1
> print(teste)
[1] 1

> teste<-2
> print(teste)
[1] 2

> 3->teste
> print(teste)
[1] 3
```

# Operadores

Os operadores na programação R são categorizados em 5 categorias diferentes:

* **Operadores aritméticos**
    

> adição `+`

> subtração `-`

> divisão `/`

> multiplicação `*`

> exponencial `^`

```c
a = 20
b = 10
c = a+b
d = a/b
e = a*b
f = a-b
g = a%%b
h = a%/%b

print(c)
print(d)
print(e)
print(f)
print(g)
print(h)
```

* **Operadores Relacionais**
    

Este mostrará o resultado em booleano, True ou False.

> Menor que `<`

> Maior que `>`

> Igual a `==`

> Diferente de `!=`

> Menor que igual a `<=`

> Maior que igual a `>=`

```c
a<b
a>b
a==b
a!=b
a<=b
a>=b
```

* \*\* Operadores lógicos\*\*
    

> `&` - Operador AND

> `|` (pipe)- Operador OR

> `!` \- Operador NOT

AND `&`: O operador AND significa verificar ambos os valores. Se algum deles for FALSE, retornará como FALSE.

OR `|`: O operador OR significa verificar qualquer um dos valores. Se algum deles for TRUE, ele retornará como TRUE.

NOT `!`: O operador NOT apenas retornaria o oposto do valor. Se o valor for TRUE, retornará FALSE e vice-versa.

```c
> a = 20
b = 0

a&b
a|b
!a
[1] FALSE
[1] TRUE
[1] FALSE
>
```

No caso da operação `&`, tanto a como b devem ser diferentes de zero. Portanto, a saída é FALSE.

No caso da operação ‘`|`, apenas um deve ser Diferente de Zero. Portanto, a saída é TRUE.

No caso da operação `!`, a saída será o oposto do seu valor. Portanto, a saída é FALSE.

Não podemos executar o tipo AND `&` em data types Double ou Character.

* **Operadores de Atribuição**
    

> Atribuição à esquerda: `<-`, `<<-` e `=`

> Atribuição à Direita: `->` e `->>`

* **Operadores Especiais**
    

> Sequência `:`

> Procurar valor no vetor `%any%`

# Funções

Procedures ou rotinas são chamadas de functions.

Há 2 tipos:

* \*\* *Built-in functions* \*\*
    

Já armazenadas em memória, como a `print()` por exemplo.

Outros exemplos de functions built-in:

function `sum()` soma:

```c
# sum function
sum(a,b)

# Ou
print(a+b)

# Ou
c = a+b
print(c)
```

function `sqrt()` raiz quadrada:

```c
> sqrt(16)
[1] 4
```

function typeof() retorna qual o tipo de dado está na variavel:

```c
> a = 10
> typeof(a)

[1] "double"
```

* \*\* *Custom functions* \*\*
    

As custom functions, são criadas pelo seu código.

Estrutura da criação de uma function:

```c
   FunctionName <- Function(arguments){
       Statements....
       ....
   }
```

Exemplo BEM simples:

Criando uma função que multiplica ao quadrado o numero dado:

```c
  aoquadrado <- function(a){
  NumeroAoQuadrado <- a * a
  print(NumeroAoQuadrado)
}

aoquadrado(32)
[1] 1024
```

# IF-ELSE Conditions

```c
      if(condition){
        if statements to be executed....
      }
      else{
        else statements to be executed....
      }
```

# Loops

## while

Irá retornar a mensagem, adicionar 1 ao contador e repetir enquanto o contador for menor que 100.

```c
count <- 1

while(count < 100) {
  print(count, "print this again")
  count <- count + 1
}
```

Irá retornar a mensagem juntamente com o valor do contador, depois, adicionar 1 ao contador e repetir enquanto o contador for menor ou igual a 100.

Neste caso, para imprimir o valor do contador junto com a mensagem, ou seja, Double + Character (inteiro e string), usaremos a function `paste`:

```c
count <- 1

while(count <= 100) {
  print(paste(count, "print this again"))
  count <- count + 1
}
```

## for

Retorna todos os nomes da coluna Names. a função `c()` retorna um vetor (um array unidimensional).

```c
Names <- c("Manuel","Floriano","Prudente","Manoel","Francisco","Afonso","Nilo", "Hermes")

for(i in Names){
  print(i)
}
```

Loop for com sequência`:` (de 1 a 10):

```c
for(i in 1:10){ 
  print(i) 
  }
```

# Vector

Vector são como uma lista, mas armazenam apenas valores do mesmo tipo de dados!

No R, todos os valores variáveis são guardados em Vectores.

Assim, quando temos `a = 1`, o R armazena como um Vector com valor 1 e tamanho 1.

Agora, quando temos `x = 1 2 3 4`, o R armazena como um Vector `x` com os valores `1 2 3 4` e tamanho 4 (lembre-se: este é um tipo Double, não há aspas duplas para ser um Character e não há L no final para ser um Inteiro).

```c
V1 <- c("Manuel","Floriano","Prudente","Manoel")

V2 <- c(1,2,3,4,5)
# Ou
V2 = 1:5
```

Para exibir apenas uma posição no vector:

```c
print(V1[3])
[1] "Prudente"
```

**Usando a function seq():**

```c
 seq(1,20) # gera uma sequência de 1 a 20
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20

 seq(1,20,2) # gera uma sequencia de 1 a 20 em saltos de 2
 [1]  1  3  5  7  9 11 13 15 17 19

 V3 = seq(1,20,2) # Cria um vetor com o resultado da fuction seq()

> print(V3)
 [1]  1  3  5  7  9 11 13 15 17 19
```

**Usando a function rep():**

```c
rep(3,5)
[1] 3 3 3 3 3

V1 = rep(3,5)

print(V1)
[1] 3 3 3 3 3
```

```c
V1 = rep("a",5)

print(V1)
[1] "a" "a" "a" "a" "a"
```

Podemos adicionar, subtrair, dividir ou multiplicar diretamente com os vectores.

```c
v1 = c(8,7,5,3,6)
v2 = c(4,7,1,0,9)
 
add.vector = v1+v2
print(add.vector)
[1] 12 14  6  3 15

subtract.vector = v1-v2
print(subtract.vector)
[1]  4  0  4  3 -3

multiply.vector = v1*v2
print(multiply.vector)
[1] 32 49  5  0 54

divide.vector = v1/v2
print(divide.vector)
[1] 2.0000000 1.0000000 5.0000000       Inf 0.6666667
```

Baseados na posição do vetor:

```c
v1 = c( 8 , 7 , 5 , 3 , 6 )
       |   |   |   |   |
v2 = c( 4 , 7 , 1 , 0 , 9 )
```

Se o segundo vetor tiver menos dados, o R irá repetir a sequência, veja no exemplo abaixo:

```c
v1 = c(8,7,5,3,6,7)
v2 = c(4,7)

add.vector = v1+v2
print(add.vector)
[1] 12 14  9 10 10 14
 
subtract.vector = v1-v2
print(subtract.vector)
[1]  4  0  1 -4  2  0
 
multiply.vector = v1*v2
print(multiply.vector)
[1] 32 49 20 21 24 49

divide.vector = v1/v2
print(divide.vector)
[1] 2.0000000 1.0000000 1.2500000 0.4285714 1.5000000 1.0000000
```

Ou seja, `v1 = c(8,7,5,3,6,7)` manterá, mas o `v2 = c(4,7)` será repetido, virando `v2 = c(4,7,4,7,4,7)`

# Matrix

Os vetores são um grupo de elementos de um único tipo de dados e que representam os dados numa única dimensão.

Isto significa que terá "1 linha e n colunas" que podem ser escritas como `(1,n)`

A matriz é semelhante a um Vector mas é bidimensional , contém 'm linhas e n colunas' que podem ser escritas como `(m,n)`.

A matriz é definida como um grupo de elementos semelhantes dispostos em duas dimensões.

INDEXAMENTO é como podemos acessar cada elemento de uma Matriz.

A matriz é feita usando uma combinação de números de linhas e números de colunas dentro de colchetes, por exemplo: `[linha1,col3]`

Matrix pode ser criada usando 3 functions: matrix() # matrix function rbind() # Row function cbind() # Column function

Exemplo:

vector com 20 valores:

`vec = c(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20)`

Matrix com 4 linhas e 5 colunas e os valores do vector `vec`.

```c
vec = c(1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20)

M1 = matrix(vec,4,5)

print(M1)
     [,1] [,2] [,3] [,4] [,5]
[1,]    1    5    9   13   17
[2,]    2    6   10   14   18
[3,]    3    7   11   15   19
[4,]    4    8   12   16   20
```

Ou, de outra forma:

```c
M2 = matrix(c(1:20),4,5)
print(M2)
```

Ou, criando uma Matrix com diversos vetores:

```c
v1 = c(1,5,9,13,17)
v2 = c(2,6,10,14,18)
v3 = c(3,7,11,15,19)
v4 = c(4,8,12,16,20)

M3 = rbind(v1,v2,v3,v4)

print(M3)
   [,1] [,2] [,3] [,4] [,5]
v1    1    5    9   13   17
v2    2    6   10   14   18
v3    3    7   11   15   19
v4    4    8   12   16   20
```

Criando um Vector usando posições diferentes de diversas Matrix, usando o `rbind` para a mesma quantidade de itens na linha.

Isto irá criar um Vector de 3 linhas e 4 colunas com os 12 valores presentes nos 3 Vetores.

```c
M4 = rbind(c(1:4),c(5:8),c(9:12))

print(M4)
     [,1] [,2] [,3] [,4]
[1,]    1    2    3    4
[2,]    5    6    7    8
[3,]    9   10   11   12
```

Criando um Vector usando posições diferentes de diversas Matrix, usando o `cbind` para o mesmo tamanho de coluna.

Isto irá criar um Vector de 5 linhas e 4 colunas com os 20 valores presentes nos 4 Vetores.

```c
M5 = cbind(v1,v2,v3,v4)

print(M5)
     v1 v2 v3 v4
[1,]  1  2  3  4
[2,]  5  6  7  8
[3,]  9 10 11 12
[4,] 13 14 15 16
[5,] 17 18 19 20
```

# Data frames

Com o Data Frame, podemos armazenar qualquer tipo de valor/dados.

Estrutura:

```c
DataFrame_Name = data.frame(
   col1_name = col1_values,
   tcol2_name = col2_values,
   coln_name = coln_values
   )
```

Chamando o Data frame como function:

`data.frame()`

Vamos criar um Data Frame com 4 colunas: ID, Nome, Idade e Departamento, com 5 valores presentes.

```c
List = data.frame(ID = 101:105,
           Nome = c("Mike","Henry","Tom","Harvey","Rachel"),
           Idade = c(21,22,21,20,22),
           Department = c("IT","EX","EX","CS","IT")
)

print(List)

   ID   Nome Idade Department
1 101   Mike    21         IT
2 102  Henry    22         EX
3 103    Tom    21         EX
4 104 Harvey    20         CS
5 105 Rachel    22         IT
```

# List

Listas são algo em que podemos armazenar Valores, Vetores, Matrizes e Data Frame, todos juntos num único arquivo.

Dentro da função, podemos adicionar qualquer número de Vetores, Matrizes ou Data Frames, assim como podemos declarar Variáveis únicas.

Usa-se a function `list()`

Como:

`List_Name = list(vectorName1, MatrixName1, DataFrameName1, vectorName2,...)`

Um exemplo:

```c
#Data Frame
DataFrame1 = data.frame(ID = 101:105,
           Nome = c("Mike","Henry","Tom","Harvey","Rachel"),
           Idade = c(21,22,21,20,22),
           Department = c("IT","EX","EX","CS","IT")
)

#Vector
V<- 1:4;

#Matrix
M <- matrix(1:16,4,4)

#variable
Num <- 4

#Uma lista com todos:
List = list(V, M, Num, DataFrame1)

# exibindo a lista:


print(List)

[[1]]
[1] 1 2 3 4

[[2]]
     [,1] [,2] [,3] [,4]
[1,]    1    5    9   13
[2,]    2    6   10   14
[3,]    3    7   11   15
[4,]    4    8   12   16

[[3]]
[1] 4

[[4]]
   ID   Nome Idade Department
1 101   Mike    21         IT
2 102  Henry    22         EX
3 103    Tom    21         EX
4 104 Harvey    20         CS
5 105 Rachel    22         IT
```

# Packages

Os pacotes são extensões do R. Os pacotes contêm código, dados e documentação em um formato de coleção padronizado.

O CRAN é o repositório padrão onde ficam os packages, semelhante ao que é o Pip para o Python, Cpan para o Perl...

* Vamos testar o package **ggplot2**.
    

ggplot2 é um sistema para criar gráficos declarativamente, baseado na gramática dos gráficos. Você fornece os dados, informa ao ggplot2 como mapear variáveis ​​para estética, quais primitivas gráficas usar e ele cuida dos detalhes.

Instalar o package:

`install.packages("ggplot2")`

Após (demora um pouco!), vamos carregar o package no nosso ambiente atual:

`library(ggplot2)`

Por fim, executa-lo com um exemplo simples (da própria doc):

```c
ggplot(mpg, aes(displ, hwy, colour = class)) + geom_point()
```

Resultado:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661294438100/4KctkAd5g.png align="left")

Aqui a documentação completa deste package:

https://www.rdocumentation.org/packages/ggplot2/versions/3.3.6

Um Cheatsheet do ggplot2:

https://github.com/rstudio/cheatsheets/blob/main/data-visualization-2.1.pdf

Pagina oficial do ggplot2:

https://ggplot2.tidyverse.org/

* Outro package, o **txtplot**:
    

No console do R:

`install.packages('txtplot')`

O txtplot é uma biblioteca que gera gráficos ASCII, incluindo gráficos de dispersão, plotagem de linhas, plotagem de densidade, acf e gráficos de barras.

Depois de instalado, podemos carrega-lo e rodar a chamada abaixo para gerar um exemplo:

```c
>  library('txtplot')

> txtplot(cars[,1], cars[,2], xlab = "velocidade", ylab = "distancia")
      +----+-----------+------------+-----------+-----------+--+
  120 +                                                   *    +
      |                                                        |
d 100 +                                                   *    +
i     |                                    *                *  |
s  80 +                          *         *                   +
t     |                                       * *    *    *    |
a  60 +                          *  *      *    *      *       +
n     |                        *         * *  * *              |
c  40 +                *       * *    *  *    * *              +
i     |         *      *  * *  * *  *                          |
a  20 +           *    *  * *       *                          +
      |  *      *    *                                         |
    0 +----+-----------+------------+-----------+-----------+--+
           5          10           15          20          25   
                             velocidade                         
>
```

# Rstudio, VSCode e Terminal

Todos os exemplos aqui foram feitos via RStudio, vscode (com as extensões R) e no terminal do Linux.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661392536862/nw2UV5RW1.png align="left")

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661392835718/_FyT2apGj.png align="left")

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1661392912867/d3PJruO73.png align="left")

# Links

RDocumentation (pesquisa a documentação dos packages e functions): https://www.rdocumentation.org/