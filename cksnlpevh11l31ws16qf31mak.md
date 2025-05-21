---
title: "Grep e Regex"
datePublished: Sun May 15 2016 19:25:16 GMT+0000 (Coordinated Universal Time)
cuid: cksnlpevh11l31ws16qf31mak
slug: grep-e-regex
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1629659631932/HvftVb9z-.png
tags: regex

---

Neste artigo, vamos ver alguns parâmetros do grep aplicando/usando operadores lógicos nas buscas (como 'e', 'ou' - melhor dizendo: 'and', 'or') e também uma pequena introdução no uso de expressões regulares (regex/regexp).


## História do grep

Por um bom tempo, o grep foi um utilitário particular e usado apenas pelo seu criador (Ken Thompson, o criador do Unix), e que quase foi descartado. O grep surgiu como um aplicativo independente adaptado de um analisador de expressão regular que ele havia escrito para o editor de texto 'ed' (que ele também criou). No ed, o comando g/re/p imprimia todas as linhas que correspondem a um padrão previamente definido.
Significado: g/re/p (global regular expression print)



Finalizamos a história, não vou mostrar como instala, porque, se vc está lendo isto, creio que sabe como instala-lo em seu Linux e provavelmente sabe mais ou menos como ele funciona ou como usa (ou já copiou e colou no terminal algum comando com grep, mesmo sem saber para que ou como).



## GREP
O pacote grep instala além do próprio grep, o egrep e fgrep.
o egrep é a mesma coisa que 'grep -E', fgrep tem o mesmo retorno no 'grep -F' e o rgrep é a mesma coisa que 'grep -r'.

 
Fazendo uma pesquisa simples, encontramos também pacotes como os:

- **sgrep** - grep para arquivos estruturados (por exemplo, num arquivo de alguma linguagem, ele pode mostrar todo conteudo dentro de um bloco que seja compativel com a pesquisa)
- **ngrep** -  grep para rede, quando bem aplicado, torna-se melhor que as comumente usadas tcpdump e afins (tshark, ...)
- **mboxgrep** e **grepmail** - grep aplicado em caixa de email
- **agrep** - pesquisa por strings e expressões regulares dentro de um arquivo
- **ack-grep** - ack é uma alternativa ao grep, na descrição, diz que foi feito para programadores.


Vamos trabalhar com o arquivo */var/log/debug*.
Eu preciso saber das entradas de log que tiveram a linha mensionando o daemon "Watchdog".

Exemplo 1:

```
grep Watchdog /var/log/debug
``` 

Isto irá me retornar todas as linhas que aparecem esta palavra no arquivo. Mas se não estiver com o primeiro carácter em maiúsculo? Ou, se eu não souber como ele está dentro do arquivo (ou se está em mais de um formato diferente)? use o "-i" para ignorar o case sensitive.

Exemplo 2:
``` 
grep -i watchdog /var/log/debug
``` 






## Operador lógico 'ou'/'or'

Descrição do operador: Operador lógico no qual a resposta da operação é verdade se pelo menos uma das condições passadas for verdade. O famoso "Ou isso ou aquilo."
Você precisará usar o backslash "\" (barra contra) e posteriormente o pipe "|", escapando-o de ser interpretado por causa do \.


Exemplo 1:

Além de pesquisar o Watchbod, preciso ver as linhas referentes ao utilitário chroot
```  
grep -i 'watchdog\|chroot' /var/log/debug
``` 


Exemplo 2:

Se você usar o '-E' para expressões regulares estendidas, não precisa escapar o pipe "|" com o backslash "\":

``` 
grep -i -E 'watchdog|chroot' /var/log/debug
``` 

Exemplo 3:

Lembra do egrep? então:
``` 
egrep -i 'watchdog|chroot' /var/log/debug
``` 


Exemplo 4:

A opção "-e" no grep é para definir um (1) parametro de expressão regular, mas não há impedimentos que impossibilite o uso de vários "-e" no comando:

```  
grep -e watchdog -e chroot /var/log/debug
``` 





 ## Operador lógico 'e'/'and'

Descrição do operador: Operador lógico no qual a resposta da operação é verdade se ambas as variáveis de entrada forem verdade.


No caso do grep, não há algo que realmente faça isto, mas a busca com esta lógica pode ser feita de algumas formas, onde será mostrado ao grep duas ou mais condições e o resultado obtido deve obedecer TODAS as condições.


Neste caso, há vários modos de se fazer o 'and' com o grep. O que mais vejo os usuários é direcionando o resultado de um grep para um segundo grep, tendo somente os resultados que obedecem as duas condições.
Como meu debug trouxe muitos resultados e a minha anlise é referente ao mês de Maio, preciso filtrar os logs obedecendo a informação do mês.


Exemplo 1:
```
 grep -i May /var/log/debug | grep -i Watchdog
``` 

Pesquisando dentro do arquivo debug (em /var/log) primeiramente todas as linhas que tenham 'May' e dentro do resultado deste grep, outro grep que busca apenas as linhas que contenham a palavra 'Watchdog'.

Exemplo 2:

A mesma busca anterior, usando o '-E', pode-se usar também '--extended-regexp' e significa que é uma expressão regular estendida.

``` 
grep -i -E 'May.*Watchdog' /var/log/debug
``` 


A ordem deve ser obedecida, ou seja:

Irá buscar as linhas que tiver 'May' e depois, dentre estes, procurar o que há 'Watchdog'. Se o 'Watchdog' estiver escrito na linha antes do May, esta linha não aparecerá nos resultados. 
Como neste exemplo estamos trabalhando com o arquivo /var/log/debug e todo ele começa com a data (como: " May 14 10:33:24 "), perceba que, se você inverter as palavras na busca, não haverá resultados no grep.


 
Exemplo 3:

Lembra que o egrep significa 'grep -E'? Então podemos fazer também: 

```  
egrep -i 'May.*Watchdog' /var/log/debug
``` 

 
Pesquisa com grep usando AND + OR (e + ou)

Preciso de 2 pesquisas:
A primeira, saber se houve algum registro nos logs do debug relacionados a aplicação "Watchdog", e depois, saber se houve algo com o "chroot", também no mês de Maio. 
Porém em ordem cronológica, ou seja, mostrar as ocorrências numa mesma saída de comando e mostrando-as do começo ao fim do mês independente de quais que sejam.

``` 
grep -i -E 'May.*Watchdog|May.*chroot' /var/log/debug
``` 

Lembrando nestes casos: 

Como a ordem para a busca (e construção da sequência de dados no comando) deve ser obedecida, você precisa saber (e conhecer) qual o resultado que espera obter, e como ele está inserido no arquivo. 
Pois, se deixar o May como segunda informação desta regex no grep, nada irá aparecer, pois todas as linhas iniciam com a abreviação do mês e ele irá buscar linhas que possuam aqueles conjuntos de caracteres (no caso o 'May') e depois de encontrar as linhas que possuam esta característica, pesquisa linhas onde, após este conjunto encontrado, encontre o conjunto de caracteres seguinte.






 ## Operador lógico 'NOT'/'não'

Descrição do operador: Operador lógico que representa a negação (inverso) da variável atual.

Exemplo 1
Mostra todas as linhas do arquivo debug, exceto as que contém a palavra "Watchdog": 

``` 
grep -v 'Watchdog' /var/log/debug
 ``` 

Exemplo 2: 

Como o meu arquivo debug está com os dados de Abril e Maio (Apr e May), posso realizar as mesmas buscas dos exemplos anteriores, porém ao invés de inserir na regex para buscar pelo 'May', podemos faze-lo ao final, excluindo o que for do mês de Abril (Apr). Lembrando que, conheço meu arquivo de log e tenho a certeza de que há nele, apenas os meses de Maio e Abril.

 
``` 
grep -i -e 'Watchdog' /var/log/debug | grep -v 'Apr'


grep -i 'Watchdog\|chroot' /var/log/debug | grep -v 'Apr'


grep -i -E 'Watchdog|chroot' /var/log/debug | grep -v 'Apr'
 ``` 



 
## Exemplos e parametros do grep + regex

Pesquisando caracteres especiais no grep:

É necessário 'escapar' o caractere usando o backslash (\), como exemplo:

``` 
grep '\'aulas' planejamento.txt 
``` 

Neste caso, a pesquisa retorna as linhas onde possuem: 'aulas


Pesquisando espaço no grep

Mesma solução do exemplo acima:

```  
grep '\'aulas\ de\ ITIL\'' planejamento.txt 
``` 


Neste caso, será retornado onde o grep encontrar: 'Aulas de ITIL' no arquivo.

Pesquisando em todos os arquivos:

usando asterisco:

``` 
grep '\'aulas' * 
``` 


Ou também, usando o "-r" (recursive) e o "-R" que além de recursivo, busca também nos links simbolicos: 

``` 
 grep -r '\'aulas' 

 grep -R '\'aulas' 
``` 
 
Busca algo que esteja exatamente no inicio da linha:
```  
grep "^May 14" /var/log/debug.log 
``` 

Busca algo que esteja exatamente no final da linha:
``` 
grep "running.$" /var/log/debug.log
``` 

Buscando linhas que contenham exatamente o parâmetro dado:
``` 
grep "^Indice$" planejamento.txt
``` 
 

Buscando linhas vazias com o grep (para que isto?):
```  
grep "^$" /var/log/debug.log 
``` 

Contanto quantas linhas vazias há no arquivo:
``` 
grep -c "^$" /var/log/debug.log 
``` 
 

Finalizando:
Falar sobre uso do grep e regex é chover no molhado! A quantidade de artigos, exemplos e macetes na internet é gigante, bem como publicações impressas e sites dedicados a isto.


 Recomendações:
http://www.regular-expressions.info/grep.html

http://www.gnu.org/software/grep/manual/grep.html


Aurelio Jargas:
http://aurelio.net/regex/
E seu excelente livro sobre REGEX:
http://piazinho.com.br/


Saiba mais sobre a história do grep neste link: https://medium.com/@rualthanzauva/grep-was-a-private-command-of-mine-for-quite-a-while-before-i-made-it-public-ken-thompson-a40e24a5ef48#.r3hvtf3zb