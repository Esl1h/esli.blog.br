---
title: "Iperf e Jperf- Teste de Performance de Rede"
datePublished: Fri Feb 11 2022 22:22:27 GMT+0000 (Coordinated Universal Time)
cuid: ckziz6k340e2yt1s1hqtg5odx
slug: iperf
canonical: https://www.esli-nux.com/2012/08/teste-de-performance-de-rede-com-iperf.html
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/40XgDxBfYXM/upload/v1644618058641/GNN4RCy9J.jpeg
tags: network, performance, server

---

## Base de Conhecimento

O TCP é o protocolo da camada de transporte orientado à conexão, que oferece um serviço confiável. Frequentemente aparece como parte da pilha TCP/IP da arquitetura Internet, mas é um protocolo de propósito geral que pode ser adaptado para ser usado com uma variedade de sistemas.

O protocolo UDP é restringido a portas e sockets, e transmite os dados de forma não orientada à conexão. Ele nada mais é do que uma interface para o protocolo IP. Esse protocolo substitui o protocolo TCP quando a transferência de dados não precisa estar submetida a serviços como controle de fluxo. A função básica do UDP é servir de multiplexador ou demultiplexador para o tráfego de informações do IP. Assim como o TCP, trabalha com portas que orientam adequadamente o tráfego de informação a cada aplicação de nível superior.

O UDP fornece os serviços de broadcast e multicast, permitindo que um único cliente envie pacotes para vários outros na rede. O UDP é uma escolha adequada para fluxos de dados em tempo real, especialmente aqueles que admitem perda ou corrompimento de parte de seu conteúdo, tais como vídeos ou voz. Aplicações sensíveis a atrasos na rede, mas poucos sensíveis a perdas de pacotes, como jogos de computadores, também podem se utilizar do UDP. As garantias de TCP envolvem retransmissão e espera de dados, como consequência, intensificam os efeitos de uma alta latência de rede. Algum tratamento de erro pode ser adicionado, mas geralmente aplicações multicast também admitem perda de parte dos pacotes ou fazem retransmissões constantes (tal como o ocorre no protocolo DHCP).

O UDP não perde tempo com criação ou destruição de conexões. Durante uma conexão, o UDP troca apenas 2 pacotes, enquanto no TCP esse número é superior a 10.

Embora o processamento dos pacotes UDP seja realmente mais rápido, quando as garantias de confiabilidade e ordenação são necessárias, é pouco provável que uma implementação em UDP obterá resultados melhores do que o uso direto do TCP. Em primeiro lugar, corre-se o risco de que uma implementação ingênua feita em UDP seja menos eficiente do que os algoritmos já testados e otimizados presentes no TCP. Em segundo lugar, boa parte do protocolo TCP, nos principais sistemas operacionais, opera em modo núcleo, tendo portanto uma execução mais privilegiada do que um protocolo de aplicação teria. Finalmente, é válido lembrar que muitas placas de rede já possuem o TCP programado em seu firmware o que permite, por exemplo, o cálculo de checksum por hardware. Por isso, o protocolo UDP não deveria ser utilizado para fluxos de bytes confiáveis, tais como a transferência de arquivos. Como exemplo, podemos citar o protocolo TFTP, exceção a essa regra, que foi feito redes locais, de alta confiabilidade. Na internet, seu desempenho é muito inferior ao sua versão confiável, o protocolo FTP.

De uma forma bem simplificada, A principal diferença entre ambos, é que o tcp é orientado a conexão, ou seja, antes de enviar os dados é feito uma comunicação entre remetente e destinatário, cria-se um canal de comunicação, então é transmitido os dados... Já o udp não é orientado a conexão, portanto os dados são enviados sem ter a certeza de que o receptor recebeu os dados... Isto mostra alguns pontos conclusivos entre TCP e UDP, o primeiro mais seguro, onde há um tratamento sobre perdas e conexão, já o segundo é mais rápido e com melhor aplicabilidade em determinadas situações (as vezes necessárias).


- Quem usa TCP: Telnet, FTP, SMTP, SSH, HTTP,...

- Quem usa UDP: SNMP, TFTP, RPC, DHCP,...


O TCP, tal como o UDP, usa o IP para a entrega dos datagramas à rede, e os pontos de acesso à aplicação são identificados por portas acessadas por multiplexação, tal como acontece com o UDP, o que permite múltiplas ligações em cada host. As portas podem ser associadas com uma aplicação (Processo).

O IP trata o pacote TCP como dados e não interpreta qualquer conteúdo da mensagem do TCP, sendo que os dados TCP viajam pela rede em datagramas IP. Os roteadores que interligam as redes apenas verificam o cabeçalho IP, quando fazem o envio dos datagramas. O TCP no destino interpreta a mensagem do protocolo TCP.



## Rede Local e o tráfego de informações

Nas comunicações que ocorrem em redes locais Ethernet o endereço IP não é diretamente usado na comunicação, ele serve apenas para se descobrir o MAC Address, que é de fato o endereço que importa nas comunicações locais. Essa descoberta é feita através de um protocolo de rede chamado Address Resolution Protocol (ARP). De forma resumida, o switch fica responsável por registrar cada interface MAC dos hosts conectados e enviar os pacotes aos hosts de destinos, no caso de um hub, ele não possui este mapeamento de MAC address, o pacote é enviado a todas as maquinas.

Jitter é definida como a medida de variação do atraso entre os pacotes de dados. Observa-se ainda que uma variação de atraso elevada produz uma recepção não regular dos pacotes. Logo, uma das formas de minimizar a variação de atraso é a utilização de buffer, o qual armazena os dados à medida que eles chegam e os encaminha para a aplicação seguindo uma mesma cadência.

Buffer é uma região de memória temporária utilizada para escrita e leitura de dados. Normalmente são utilizados quando existe uma diferença entre a taxa em que os dados são recebidos e a taxa em que eles podem ser processados, ou no caso em que essas taxas são variáveis.




## O que é
O Jperf consiste em um software de análise de performance de banda e cálculo de perda de datagramas na rede, cria fluxo de dados sob TCP e UDP e permite medir e analisar o desempenho da rede. Ele é mantido pela Universidade de Illinois. O Iperf é um software livre, do tipo client/server desenvolvido pelo National Laboratory for Applied Network Research (NLANR), é escrito em C++ e sua última versão é a 2.0.5, disponibilizada em Julho/2010 (nota: há um projeto onde desenvolve-se a continuação de um “novo” iperf, atualmente a versão é 3.0-beta4).

É uma ferramenta multi-plataforma que pode ser executada através de qualquer tipo de rede (falo sobre topologias, infraestrutura, meios, etc...) e exibir resultados de desempenho padrão com níveis em comum entre essas diferentes redes, assim, ele pode ser usado para a comparação de equipamentos de rede com e sem fio e tecnologias de uma maneira imparcial.

Com o Iperf podemos testar/medir o throughput da rede, serve como ferramenta de apoio para outros testes em rede, simulação de conectividade, situação/diagnóstico da situação do cabeamento (para certificar sua rede em relação a atividade e transmissão de dados, identificar segmentos da rede com falhas...).

A função exercida pelo Iperf também é alcançada utilizando alguns softwares no linux com propositos de “network tools”, como scanners, sniffers e outros de monitoramento. O diferencial é que nesses aplicativos o administrador teria pouca informação (como um simples ping) ou, num software mais robusto, teria que “peneirar” a informação desejada dentro de uma inundação de logs gerados, o Iperf é justamente a aplicação que irá dar a informação desejada. Outros softwares semelhantes ao Iperf são: ttcp (test tcp) e bwping.

Veja no final do tutorial os links para downloads, versões do iperf e jperf além de outros links ;-)


## Instalação no GNU/Linux:

Depois de ter baixado o aplicativo descompacte e compile.

```
root@server ~ # tar -zxvf iperf-2.0.5.tar.gz
root@server ~ # cd iperf-2.0.5
root@server ~ # ./configure ; make ; make install

``` 




Seu funcionamento é simples, na configuração padrão, o cliente passará a enviar tráfego TCP para o servidor por 10 segundos, e em seguida mostrará a quantidade dados transferida (MBytes) e a velocidade atingida (Mbits/s).
Deve haver no minimo duas maquinas com o Iperf em sua rede. Uma será executada como servidor a outra como cliente. Por padrão o iperf usará a porta TCP 5001, certifique-se que não há nenhum tipo de firewall bloqueando esta porta (tanto nas maquinas cliente/servidor) ou na rede.





## Possíveis situações de uso


Bem, aqui a coisa complica ;-)

Podemos criar alguns ambientes simulando possíveis situações para o uso do iperf, mas pode ficar meio confuso, pois os resultados obtidos em softwares mais complexos são mais aproveitáveis e fáceis de se adquirir do que com o iperf, para qualquer administrador de rede.

O primeiro caso é utilizado por mim (e sempre aconselho a utilizarem), é a utilização para fins acadêmicos (lembra que ele é mantido por uma Universidade??), o iperf é uma ótima ferramenta para ensino. Leva o aluno a usar um sistema operacional, testar numa rede real, ao invés de simuladores de rede com softwares complexos para ministrar conteúdos simples.

Inserindo em um ambiente corporativo: Como provar para um superior dentro da empresa que um determinado segmento da rede, uma sala, andar precisa de uma nova infraestrutura de rede, novos cabeamentos, que precisa de melhorias?? Você não vai levar um diretor para ficar vendo canaletas, cabos utp empoeirados, racks e guias?? No mundo corporativo ninguém sabe a diferença entre fibra óptica e coxial, o que importa sãos números, o impacto financeiro que causa na receita, na margem... Com o iperf você coloca o iperf server num ponto em comum (um servidor da rede, firewall, dmz, qualquer ponto onde todos os hosts devem acessar), e executa os testes com o iperf cliente a partir de um ponto de rede “bom” e outro a partir do local onde com certeza é problemático. Você terá conteúdo suficiente para produzir uma boa documentação (seguindo todos os processos gerenciais, administrativos, e bla-bla-bla), obviamente com argumentos e teorias/normas de cabeamento estruturado e vários outros textos bonitos e acadêmicos em sua documentação, o iperf entra como ponto chave (cereja do bolo) concluindo e comprovando tudo, sem possibilidades de dúvidas (dica: diretores querem ver gráficos, imagens e cores, dificilmente irá ler sua documentação, mas você comprovará que parte da rede compromete o funcionamento da comunicação, que no final das contas afeta o faturamento). Para quem não é de TI, textos (mesmo que simples) são horríveis (por preconceito e preguiça, já acham que não vão entender o que está escrito). Com isto, o sucesso de um projeto aprovado são gráficos e imagens bem colocadas e o esclarecimento de como afeta no faturamento (infelizmente a maioria entende TI como gastos e despesas, e profissional de redes como um “pica-fio”).

Um terceiro e último bom exemplo, digamos que você topou fazer uma pequena rede num escritório, como um bom estudante/profissional de redes, você fez tudo pelas normas, bonitinho, cabeamento estruturado, um mini-rack de parede, canaletas, keystone,... No final, como as boas empresas de infraestrutura, você entrega toda a documentação de como fez, por que fez, o que usou, como/onde/por que... Para certificar sua rede, mostrar todo o poder de transmissão e conexão, nada como uns bons logs do iperf, screenshots, e se a pessoa pagar a vista, adiantado e com uns trocados para o cafezinho, você faz testes ao vivo, em cores, e mostrando ao seu cliente que cada centavo investido valeu ;-).

Claro, pensei em 3 situações bem distintas e fáceis de se encontrar por aí, mas há vários outros ambientes onde podemos inserir o iperf com muito sucesso (se você usou-o com sucesso e retornos positivos, dê um feedback)...



O Básico - Executando como Server
No Windows

Via linha de comando (Iniciar > Executar > digite “cmd”) entre na pasta onde o Iperf foi salvo (neste exemplo coloquei dentro de uma pasta chamada “iperf” em Arquivo de programas).

Ao acessar a pasta onde está o iperf (use o comando “dir” para ter certeza, ele vai listar o que há dentro da pasta). Para iniciar o iperf como servidor apenas digite: “iperf –s”.


```
C:\ Arquivos de programas\Iperf > iperf –s
```

O iperf será iniciado como servidor e está aguardando conexões dos clientes (lembre-se da porta padrão).

![manual iperf e jperf_html_34230074.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617031683/_vU_LRkSs.png)




## No GNU/Linux

Considerando que a porta TCP 5001 esteja habilitada, entre com o comando abaixo em um terminal para o Iperf subir como o servidor:
```
root@server ~ # iperf
```


Aparecerá a seguinte resposta:

```
root@server ~ # iperf -s
------------------------------------------------------------
Server listening on TCP port 5001
TCP window size: 85.3 KByte (default)
------------------------------------------------------------
```
Após executar o iperf como servidor, mantenha a janela do terminal aberta. Caso queira certificar-se que a porta está aberta e recebendo conexões, digite seguinte comando em outro terminal:
```
root@server ~ # nmap 127.0.0.1 -p 5001 | grep 5001
```


O nmap irá responder da seguinte forma:
```
root@server ~ # nmap 127.0.0.1 -p 5001 | grep 5001
5001/tcp open commplex-link
```


Nesta resposta do nmap há a seguinte informação:

PORTA(5001/tcp) STATUS(open) e SERVIÇO (commplex-link)

Neste comando que inseri, apenas pedi para que o nmap desse um “Olá” na minha maquina (127.0.0.1 ou localhost) na porta especificada (-p 5001). O resto foi apenas enfeite, a continuação ( | grep 5001 ) faz com que apenas seja exibido a linha onde contém este número (5001).

Verifique também que após realizar este comando com o nmap, no terminal onde está rodando o servidor iperf, ele deu uma “movimentada”:

```
[ 4] local 127.0.0.1 port 5001 connected with 127.0.0.1 port 44382
[ ID] Interval Transfer Bandwidth
[ 4] 0.0- 0.0 sec 0.00 Bytes 0.00 bits/sec
```






## O Básico – o Cliente
No Windows
Numa segunda maquina, navegue pelo prompt até a pasta onde está executável do iperf, digite o comando para executá-lo como cliente:



```
C:\Program Files\Iperf > iperf –c {IP.DO.IPERF.SERVER}
```


## No Linux

Para executar no Linux, basta digitar o comando, pois o iperf foi instalado no linux (esta é a diferença entre a versão do Windows e do Linux, no Windows não há instalação, sendo que você deve navegar pelo prompt até a pasta onde está o executável).

Neste exemplo iremos considerar que 172.22.254.254 seja o endereço ip de nosso servidor Iperf, e o cliente está sendo executado numa maquina com ip 172.22.0.1, a saída do comando seria algo parecido como:


![manual iperf e jperf_Windows.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617073105/zUNQiko92.jpeg)


Neste exemplo acima, foi transferido 114 MBytes em 95,2 Mbits/sec.

Na maquina cliente, por linha de comando, digitando iperf – c e o IP do Iperf Server. Isto é o suficiente para que o Iperf envie tráfego TCP do client para o server durante 10 segundo (essa é a configuração padrão). Após 10 segundos as informações são mostradas, como na imagem acima.

Neste exemplo, em 10 segundos foram transferidos 114 MBytes, atingindo a velocidade de média de 95,2 Mbits/sec (normal em uma rede 100 Mbits). Note que no server também são mostradas as estatísticas.






Utilizando UDP
Outra opção é adicionar o argumento –u nos dois lados (server e client) para que o teste seja efetuado com pacotes UDP.


Na inicialização do Servidor:
```
root@server ~ # iperf -s -u
```



No Windows:
```
C:\Program Files\Iperf> iperf -s –u
```

Como flag no cliente para informa-lo da utilização do protocolo UDP:
```
root@client~ # iperf -c 192.168.0.1 -u
```




## No Windows:
```
C:\Program Files\Iperf> iperf –c 10.10.8.75 –u
```


Usando esta opção (UDP), no final da transmissão de pacotes, quando são exibidas as estatísticas no server, aparecem mais três itens: Jitter, número total de pacotes transmitidos e pacotes perdidos.


![iperf-linux.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617108567/C4Hqwmy4u.png)




Nos mesmos 10 segundos utilizados anteriormente, tivemos 0,018 milissegundos de jitter e nenhum pacote perdido, de 893 transmitidos.

Observe que a transferência de dados foi menor, porque a taxa de transferência padrão UDP no Iperf é de 1 Mbps.

Se você quiser aumentar a banda utilize a opção –b do lado cliente:
```
root@client~ # iperf –c 10.10.8.75 –b 70M
```
No exemplo acima será tranferido 70 Mbits/sec. Este opção funciona para o modo UDP apenas.



## Argumentando... Mais Opções
Além das opções já citadas, o Iperf ainda oferece outros argumentos, que podem ser utilizados de acordo com sua necessidade. As flags são inseridas como a maioria dos aplicativos em modo texto para GNU/Linux, seguindo a linha:

Para iniciar como servidor utilizando por padrão o protocolo TCP:
iperf -s [ opções ]


Para iniciar o cliente, indica-se o ip do servidor:
iperf -c ip.do.iperf.server [ opções ]


Para iniciar como servidor utilizando UDP:
iperf -s -u [ opções ]


Para iniciar o cliente, além ip do servidor, qdeve usar a flag indicando o uso de UDP:
iperf -c -u ip.do.iperf.server [ opções ]


Se o servidor foi iniciado apenas com o padrão (-s), ou seja, usando o TCP, o cliente obrigatoriamente vai ser executado também como TCP. Caso o servido foi iniciado usando a flag -u (para UDP), o cliente deverá usar “-u” em todos os testes. Não é possível iniciar o servidor como UDP e rodar testes no cliente como TCP ou vice-versa.


## Opções gerais
As opções “gerais” são flags que podem ser utilizadas tanto no servidor quanto no cliente.
-f, --format

É o formato com que será exibido as informações, como Kbits, Mbits, Gbits, KBytes, Mbytes, Gbytes,...

No exemplo abaixo, executei o cliente com “-f Mbytes” (caso queira, “--format MBytes”), ele irá me reportar as informações em Mbytes. As informações que estarão sendo mostradas no servidor, continuam com o formato padrão (exibindo taxas de transferências em Mbits).


![ipef-linux-client.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617142482/za4-1cMqd.jpeg)



-i, --interval n
Por padrão, o iperf (tanto cliente como o servidor) irão realizar os testes durante 10 segundos e depois exibir o resultado para você. A flag “-i N” irá fazer com que o iperf exiba o status do teste a cada N intervalo de tempo que você definir. No meu exemplo, indiquei 1 segundo (-i 1), ou seja, a cada 1 segundo durante o teste, o iperf mostrará o status das transmissões.


![iperf linux interval.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617173794/3rATPj-sb.jpeg)


-l, --len N
Com a flag “-l” você define (em B, KB ou MB) o tamanho do buffer, no exemplo abaixo (o servidor estava sendo executado como UDP, flag “-u”), inseri o número 9999, como não coloquei nenhuma letra indicativa após (B, KB ou MB), ele defin e como sendo Bytes.



![iperf linux len.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617199592/18n_jGHo5.jpeg)


-m, --print_mss
Exibe o tamanho máximo do segmento (MTU - TCP/IP header).



![iperf linux MTU.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617234038/yUNretSUG.jpeg)




-o, --output <arquivo>
Redireciona as mensagens e logs do iperf (que por padrão são exibidos ao concluir os testes) pra um arquivo, basta indicar o nome do mesmo.

-p, --port n
Indica a porta a ser utilizada. Por padrão o iperf usa a 5001. Usando “iperf -s -p 8080” o servidor ficará escutando conexões na porta 8080, e nos clientes você deverá usar a flag para indicar a porta em que o cliente enviará os pacotes (exemplo, “iperf -c x.x.x.x -p 8080”).

-u, --udp
Utiliza o protocolo UDP, ao invés do padrão TCP. Caso vá utilizá-lo, todos os iperfs (o servidor e os clientes) devem usar esta flag.



![iperf linux UDP.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617261865/RAE4gYsbC.jpeg)



-x, --reportexclude


A flag -x seguida de uma ou mais das letras: C D M S V

Faz com que o iperf omita alguma informação. A letra C omitirá informação de conexão, D dados, M multicast, S de configurações/opções, V de informações do servidor. No exemplo abaixo, utilizei “-x CMSV”, ou seja, o iperf só me mostrou informações relativas aos dados (o quanto foi transmido, em qual taxa de velocidade). Esta é uma excelente flag, pois com ela você extrai exatamente a informação necessária, excluindo de seu relatório final dados que não serão úteis.



![iperf linux client report exclude.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617307923/VCr9e2Si-.jpeg)




-y, --reportstyle C
Com a flag -y, o iperf exibirá as informações no formato CSV (separados por virgula).


![iperf linux csv.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617358962/uAY9MAJ7Y.jpeg)




-w, --window n
Com TCP opção você determina o tamanho do socket buffer (ou TCP window size).


![iperf buffer.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617384376/zadDm5qrC.jpeg)



-B, --bind <host>
bind to <host>, an interface or multicast address

-M, --mss n
Configura o tamanho máximo de cada segmento de pacote TCP (MTU - 40 bytes)


-N, --nodelay
Desabilita o delay para transmissões TCP



-V, --IPv6Version
Configuração para uso do IPv6.




## Opções para o cliente
As opções a seguir são de exclusividade da maquina em que estará rodando o cliente do iperf.


-P , --parallel
Simula a carga de transmissão de um número N de clientes em paralelo. No exemplo abaixo, indiquei o número 7, ou seja, o iperf enviará conexões como se fossem 7 maquinas com iperfs clientes enviando transmissões ao mesmo servidor.


![linux iperf parallel.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617417619/v9GaQJDNI.jpeg)


-T, --ttl
O famoso “time-to-live”, neste caso para multicast ( o valor default é 1)


-n, --num
Número de byte para transmissão (inseri um número de em seguida as letras do formato, por exemplo “-n 35 KB”).



-t, --time
Tempo de transmissão dos testes (o valor default é 10 segundos). No exemplo abaixo, inseri o “-t 15” para fazer testes por 15 segundos, juntamente com a flag “-i 1” para que o iperf exiba um status a cada 1 segundo (ao invés de apenas ficar esperando, como é o padrão), também inseri o argumento para exibir os dados em Mbytes, por final, o “-u”, pois na ocasião estava rodando o servidor como udp. No final fica a seguinte linha de comando:
iperf -c x.x.x.x -i 1 -t 15 -f Mbytes -u



![iperf time.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617444325/CnB-T1IRX.jpeg)


-d, --dualtest
Apesar do Iperf enviar tráfego no sentido Client –> Server por padrão, podemos configurá-lo para que o teste seja executado nos dois sentidos simultaneamente.

Para isto, execute o Iperf Server da mesma forma (iperf –s) e do lado cliente, apenas adicione o argumento –d.
root@client ~ # iperf -c 192.168.0.1 -d
Verifique no exemplo abaixo que o iperf exibe as duas saidas:



![iperf dual.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617467573/zGDxQtuqo.jpeg)



Observe que desta vez temos duas linhas, sendo que em um sentido a transferência atingiu 94,2 Mbits/s e no outro 35,7 Mbits/s. Se somarmos as duas temos 129,9 Mbits/s (abaixo dos 200 Mbits/s nominal de uma rede full duplex…).


-r, --tradeoff
Realiza o teste nos “dois sentidos”, servidor ← → cliente, semelhante ao argumento anterior (-d), porém neste os testes nãos são feitos simultâneos, mas individualmente.


-L, --listenport
Indica a porta que será utilizada para receber os testes bidirecionais descritos acima (-d e -r)


-b, --bandwidth
#[KM] for UDP, bandwidth to send at in bits/sec (default 1 Mbit/sec, implies -u) -b Especifica a banda a ser utilizada (bandwith)

-F, --fileinput <name>
Inseri algum arquivo para ser transmitido como forma de dados.

-I, --stdin
Inseri um arquivo para transmitir como entrada padrão (stdin)



## Opções para o Servidor
Além das Opções Gerais, que podem ser utilizadas tanto no servidor quanto no cliente, o servidor possui algumas (na verdade, apenas 2)opções exclusivas:
-s, --server
Iniciar o iperf como servidor (nem conto isto como uma opção...)

-U, --single_udp
Inicia como UDP (ao invés de TCP para as transmissões como padrão)

-D, --daemon
Inicia o servidor como um daemon.


## Interface Gráfica em JAVA

Junto com o iperf, está disponibilizado para download o pacote jperf, justamente uma interface gráfica em Java.

Realizando o download do pacote e descompactado-o, haverá o jperf.bat (para executá-lo em Windows) e o pacote jperf.sh para executar em Linux.

Este arquivo sh é apenas o comando para executar o jperf.jar com os parâmetros e flags necessárias para a maquina virtual java iniciar a aplicação (comigo funcionou perfeitamente tanto no SunJava Runtime quanto no OpenJDK, mas prefiro o OpenJDK..).

Na imagem abaixo está a tela inicial quando executamos o jperf.


![jperf linux.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617504384/3xzxD-nuh.png)


Logo de inicio, o jperf mostra o campo “iperf command”, informando qual o comando em modo texto que será executado ao você escolher as opções no modo gráfico.

Logo abaixo, o modo em que o iperf será executado: Servidor ou Cliente. Veja que habilitando uma das opções, ficará disponível configurações que vemos anteriormente como flags. Por exemplo “Parallel Streams”(opção “-P” em linha de comando). Note que alterando as opções, automaticamente a caixa “command” exibe como fica as opções que você escolheu em modo linha de comando.



![jperf linux head.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617535652/s0rlYyTMT.png)


Ao lado esquerdo da janela, há as opções de configuração relativas as configurações de transmissão, pacotes, envio, entre outras.

Estão divididas em 3 blocos:



![jperf 1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617574171/PpUaERStc.png)



![jperf 2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617578666/yg4XnzITs.png)



![jperf 3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617583320/33RA33vfP.png)




Lembrando, tudo que há nos 3 blocos do lado esquerdo são as flags que apresentei anteriormente em modo de linha de comando (bloco da camada de transporte, aplicação e IP), basta prestar atenção ao nome dos botões que você perceberá o que cada botão da interface gráfica faz, como por exemplo: No bloco “Transport Layer” há os botões para ativar TCP ou UDP, ao ativá-los, abaixo é habilitado as configurações de tamanho de buffer, pacotes, formato (em kbytes, mbytes,...) e caixa para inserir os valores. No bloco “Application Layer”, há opção para o formato de saída das informações, intervalo para reportar o status dos testes, modo de teste (dual simultâneo ou individual)...

No lado direito, há a exibição dos relatórios/logs/resultados dos testes... Estes são mostrados em duas formas:

Em log, exatamente igual ao mostrado em linha de comando.
Mas em gráficos, que é o ponto mais interessante desta interface  ;-)


![jperf band.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617603900/FWOJfwvNi.png)



Abaixo da área preta (exibição do gráfico), temos o campo “output” onde será mostrado a saída que temos ao usar o iperf em modo de linha de comando, além da opção de salvar os resultados.

Lembrete: tanto os blocos a esquerda quanto a áreas do gráfico e do log são redimensionáveis (no caso dos blocos, você oculta-os), permitindo assim que você tenha maior foco ao que deseja extrair do iperf e de seus testes. (veja exemplos e explicações abaixo):

Abaixo temos duas imagens, a primeira trata-se do Jperf sendo executado como servidor, na segunda está o cliente. No cliente (veja o campo onde mostra o comando) foi configurado para usar a opção “Parallel Stream -P” que envia um número X de conexões simultaneas, no nosso caso, 10 conexões.

Veja que tanto no cliente quanto no servidor, cada conexão possui uma cor diferente, que diferencia o comportamento de cada linha no gráfico da primeira imagem (servidor) ondeé mostrado a relação de cada conexão (cada cor) com tamanho/velocidade/tempo/largura de banda/atraso.

Obs.: Ambos estão sendo rodados em Debian, usando o OpenJDK (num deles, tive problemas ao usar o Java Sun), curiosidade: Na véspera de natal as 00hrs e 12Min ;-)


![jperf clientserver.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617636895/GhdtpH58C.jpeg)



![jperf clientserver2.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1644617652283/Zd4U0U8Go.jpeg)


## Conclusão

Este pequeno tutorial/Manual e How-to desenvolvi através de 1 mês (Dezembro/2011 a Janeiro/2012) pesquisando e testando o software iperf e sua interface gráfica. A principal fonte de informações (como qualquer programa no Linux) foi sua Man-page e o seu site oficial (como qualquer outra coisa, procure sempre quem fez, depois procure quem usa, rsrsss). Obviamente tive como referencia também vários outros sites, a maioria em Inglês, pois há falta de material do Iperf/Jperf em português (isto também foi uma das coisas que me motivaram a desenvolver este material).


Tive 4 ambientes de rede para testar o iperf:

No primeiro, uma simples rede virtualizada para ter consciência do software em questão. A maquina virtual e a hospedeira eram Linux. 
Num segundo ambiente de testes, usei duas maquinas físicas (um pc e um notebook), ambos com placa de rede em Mbits, mas ligados entre sí por um cabo crossover (mal crimpado) ligado em um utp (ambos cat5e) através de uma emenda (dessas de plástico sem-vergonha vendidas em lojas como se fosse emenda “profissional”). 
No terceiro ambiente, mostrei o iperf a um colega/profissional (PEIP Informática) e testamos o jperf numa rede cabeada (megabit) com 100% das maquinas Windows. 
Por final, realizei alguns testes em minha pequena rede que tenho em casa, onde todos os equipamentos funcionam em gigabit (switch e interface das maquinas) com pach-cords cat.6 (certificados, Furukawa).

As screenshots neste manual referem-se a todos esses testes realizados, não testei-o em wirelless, mas seria interessante mostrar a diferença entre cabeado e sem fio numa mesma rede (pois wirelless possui uma boa incidência de perda de pacotes e problemas com a velocidade real/nominal devido a interferências,...).








Minha rede está lenta, e agora??


Existe pontos na qual deve-se tomar muito cuidado ao formalizar conclusões pelos logs e resultados do iperf:


Sistema Operacional: O sistema em que está executando o iperf pode conter problemas com rede, como vírus ou conflitos de programas instalados ou atualizações, demorar para processar pacotes, firewall/antivírus ativados com “sistemas de detecção” instalados,...
Rede: Sua rede pode conter switches gerenciáveis que bloqueiam determinados pacotes/protocolos ou conexões, analisadores e IDS's que podem aumentar o tempo de transmissão, controle de banda, VLAN, etc... (recomendo que conheça bem a rede antes de usar o iperf, algo nela pode afetar os resultados do iperf).

Então, antes de pensar em sua rede, faça o teste em vários hosts diferentes, não só hardware diferente, mas Sistema operacional também.

Testou em vários hosts, verificou as informações acima e continua lento???

A partir daqui inicia-se um trabalho mais profundo onde você irá utilizar sniffers, portscan e outras ferramentas mais avançadas no Linux para identificar os motivos de sua rede estar lenta.

Para isso, DICA: comece sua investigação seguindo a tabela OSI (checando os “itens” em sua rede e em seus hosts a partir da camada 1 da tabela OSI até chagar na 7ª).

Geralmente lentidão em rede é causada por cabos mal crimpados, plugs rj45 com aquela travinha quebrada, placas de rede defeituosa, cabos UTP danificados ou velhos (tanto os pach-cords quanto os usandos em backbone, manobra em switch ou pach panel), redes feitas por eletricistas ou amiguinhos pseudo-tecnicos que cobram barato (sem seguir regras e normas básicas de rede, como a curvatura do cabo, distância de geradores de interferências, entre muitas outras...), produtos de má qualidade, cascateamento ou loopback de switch, etc...

Após verificar tudo isto, vá subindo na camada OSI, outras coisas que são comuns achar por aí, é servidores emitindo muito broadcast, serviços que geram pacotes excessivos e desnecessários na rede, servidores e switchs mal configurados, etc...

Esta etapa, foge dos “poderes” do iperf, daí vai de seus conhecimentos, estudo do que está funcionando na rede e suas particularidades, utilização de sniffers e portscans, entre muitos outros aspectos. Veremos isto num futuro tutorial, correto? Vamos ver....




Download e Links:

Site Oficial:
https://iperf.sourceforge.net/


Projeto do Iperf 3.0 (atualmente beta na versão 4)
https://code.google.com/p/iperf/


Os programas semelhantes mencionados no inicio do tutorial:

TTCP:
https://www.pcausa.com/Utilities/pcattcp.htm

BWPing:
https://bwping.sourceforge.net/