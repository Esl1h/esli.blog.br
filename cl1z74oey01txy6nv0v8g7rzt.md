---
title: "DHCP - Guia Completo"
datePublished: Mon May 18 2015 16:35:07 GMT+0000 (Coordinated Universal Time)
cuid: cl1z74oey01txy6nv0v8g7rzt
slug: dhcp-guia-completo
canonical: https://www.esli-nux.com/2012/07/dhcp-guia-completo.html
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/IlUq1ruyv0Q/upload/v1649781274937/_uxvMU0sI.jpeg
tags: network, linux, server, infrastructure, windows-server

---

atualizado em 18/03/2015
![12252151561373159116mbtwms_Penguin.svg.med.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780343581/YKQH-NsQH.png)

Olá a todos, disponibilizo mais um guia ;-)
Apesar de um assunto bem fácil, sem segredos ou mistérios, o tema deste guia é DHCP Servers.

Nele, abordo o que é o dhcp, como funciona e como configurar.
A novidade neste guia é que mostro como realizar a configuração de um servidor DHCP usando roteadores "home / small office", como os famosos d-link, encore, tenda, pacific, tp-link, etc... Como criar um servidor dhcp usando equipamentos Cisco, como habilitar o DHCP Server usando a plataforma Windows (Windows Server 2003), e finalmente usando o GNU/Linux. Claro que meu foco é favorecer o uso do Linux para prover este serviço, para isto, mostro desde a configuração mais simples, até algumas avançadas, tanto em modo texto quanto as mais variadas interfaces gráficas existentes no S.O. para configurar e monitorar este simples serviço de rede.
No GNU/Linux, abordo o DHCP Server mais utilizado no mundo (da ISC), as configurações mais utilizadas, o cliente de dhcp e alguns macetes a mais...

# A Importância do DHCP

DHCP é a sigla para Dynamic Host Configuration Protocol, ou Protocolo de Configuração Dinâmica de Hosts.

O Protocolo DHCP é definido pela RFC 2131 (Request For Coments).
Além das RFC 3315 (para DHCP com Ipv6), RFC 2132 (Opções de extensões e parâmetros DHCP e BOOTP), RFC 2489 (Processo para a definição de novas opções DHCP) e RFC 1584 (interoperabilidade entre o DHCP e BOOTP).

As documentações e textos das RFCs, não só as citadas acima, mas todas as RFCs existentes estão disponíveis online, através do endereço www.ietf.org.
A função do serviço DHCP (ou de um servidor de DHCP), é atribuir números de IP para os computadores ou qualquer interface conectada em uma rede, o principal formato deste serviço, e também um dos principais motivos de sua adoção é para utilizar a forma dinâmica de endereçamento.

O DHCP é uma evolução do Bootstrap Protocol (BOOTP, descrito na RFC 951), que por sua vez, vem do protocolo RARP, para que uma máquina soubesse seu endereço de IP, era utilizado o RARP (Reverse Address Resolution Protocol), ou Protocolo reverso de resolução de endereços, definido na RCF 903, este protocolo permite que uma estação, recém inicializada com seu sistema operacional obtido através de um servidor de arquivos remoto, informasse seu endereço físico na rede, solicitando o seu endereço IP, o servidor RARP recebia esta solicitação, consulta o endereço físico da máquina na rede e respondia com o seu endereço IP correspondente. 

Seu problema era que o servidor tinha que está em cada rede, pois a mensagem de difusão solicitando o endereçamento não passava pelos roteadores.
Para solucionar este problema foi criado um protocolo alternativo de inicialização, o chamado BOOTP, que utiliza mensagem UDP, sendo assim possível sua retransmissão ou encaminhamento pelos roteadores.

O BOOTP já possui a capacidade de informar outras configurações para a maquina cliente, como o endereço do roteador (chamado gateway), máscara de sub-rede e o endereço do servidor de arquivo que contém a imagem da memória.

Porém, o principal problema no BOOTP é que somente a atribuição manual de endereços IP é possível, ou seja, o administrador deve cadastrar no servidor BOOTP, todos os endereços físicos endereço MAC (Media Access Control address) das maquinas conectadas a rede e seus respectivos endereços IP. Concluindo, portanto, que no BOOTP o endereço IP pertence a maquina cliente, o servidor apenas a informa quando solicitado.


No início da década de 90, o IETF (Internet Engineering Task Force) trabalhou em um substituto, capaz de superar as limitações do BOOTP e adicionasse recursos novos, definindo-o então como DHCP através da RFC 2131 de Maio de 1997.

Nas documentações RFC há uma tentativa de aplicar uma Interoperabilidade entre o BOOTP e DHCP, ou seja, servidores DHCP trabalhar com antigas maquinas cliente BOOTP, e servidores BOOTP com maquinas DHCP. Porém o BOOTP possui muitas limitações, comparado ao DHCP.


Claro, esta interoperabilidade é funcional, porém nem sempre ocorre. Mesmo assim, a troca dos equipamentos e o seu desuso faz com que essa interoperabilidade não seja adotada nem necessária com o passar do tempo.



## Funcionamento


Inicialmente, as configurações sobre a rede e o endereçamento são inseridas no servidor, que passa a distribuir os endereços de IP às interfaces conectadas a rede, claramente, que estas interfaces devem possuir suporte a tal serviço e estarem configuradas a solicitar o endereço a algum DHCP Server.


Além dos endereços, o servidor de DHCP pode realizar a “concessão” de outras configurações/serviços da rede, como endereços de servidores DNS, gateways, entre outros.


O DHCP usa a estrutura de Cliente/Servidor.


No Servidor, está o software que prove o serviço, onde encontramos as configurações e parâmetros e mantém o gerenciamento dos endereços atribuídos.


Já os clientes, desde que tenham suporte ao serviço, solicitam o endereço e obtém a concessão de um IP.

Esse procedimento envolve, quatro passos:

**Discover** – Quando o cliente solicita o endereçamento

**Offer** – É fornecido o endereço ao cliente

**Request** - O endereçamento é aceito

**Acknowledge** – O endereço é listado no servidor, o IP é nomeado como pertencente aquele host ou interface.


Uma negociação simples de solicitação DHCP ocorre com a troca das mensagens: DHCP Discover, DHCP Offer, DHCP Request e o DHCP Ack.

![dhcp.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780328614/a-Y1OHrCL.jpg)






![Funcionamento do DHCP.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780365986/1Gp7rZS_g.jpg)



Portas utilizadas por padrão:

- bootp server 67/tcp
- bootp server 67/udp
- bootp client 68/tcp
- bootp client 68/udp

Portas Utilizadas no DHCPv6 (para IPV6):


- dhcpv6-client 546/tcp
- dhcpv6-client 546/udp
- dhcpv6-server 547/tcp
- dhcpv6-server 547/udp





O DHCP suporta três mecanismos para a atribuição de endereços IP.

Que são eles: Atribuição automática, alocação dinâmica e alocação manual.


Em **Atribuição automática**, o DHCP atribui um endereço de IP permanente para um cliente.


Em **alocação dinâmica**, é atribuído um endereço de IP para um cliente por um período limitado de tempo, ou até que o cliente explicitamente abandone o endereço, como por exemplo, se desconecte ou desligue a maquina que recebeu este endereço. O endereço IP pertence ao servidor, ao pool de endereços disponíveis para o cliente que solicitar.


O servidor “empresta” este endereço à máquina cliente por um período de tempo previamente determinado, chamado de “lease time”, que é definido em sua configuração.


Ao decorrer deste tempo, a máquina cliente irá contatar o servidor para a renovação deste endereço, caso o servidor não esteja acessível, ao termino deste tempo a maquina cliente abandonará o endereço IP e ficará tentando a comunicação com um servidor DHCP, através de mensagens de difusão (broadcasting) na rede.

Em **alocação manual**, o endereço de IP de um cliente é atribuído pelo administrador da rede, e o DHCP é usado simplesmente para transmitir o endereço atribuído ao cliente. O endereço IP é associado ao endereço físico da maquina, o MAC.

Uma rede privada, independentemente de seu porte, estrutura ou finalidade poderá e irá utilizar um ou mais desses mecanismos, dependendo de fatores como: a política do administrador da rede, finalidade da rede, serviços sendo executados e disponibilizados na rede, tipos de maquinas e equipamentos conectados, entre outros.


A alocação dinâmica é o único dos três mecanismos que permite a reutilização automática de um endereço que não é mais necessário ou utilizado pelo cliente para o qual foi designado inicialmente.

Assim, alocação dinâmica, é particularmente útil para atribuir um endereço a clientes que estarão conectados a rede apenas por um período de tempo, permitindo assim, compartilhar um conjunto limitado de endereços de IP para esses clientes, ou então utilizar a alocação dinâmica para inserir novos clientes numa rede onde os endereços de IP estão escassos e/ou onde há diversos clientes com IPs alocados manualmente. Este mecanismo de alocação é o mais conhecido e atribui-se a ele o principal motivo e argumento para que uma rede adote o serviço de DHCP.




### Vantagens do Uso do DHCP

Com a finalidade do DHCP bem definida e descrita, serão listadas abaixo algumas vantagens em forma de tópicos, reunindo as mais importantes e de forma simplificada.
Deve-se levar em consideração que não há opções paralelas ou alternativas ao DHCP, ou seja, ou a rede utiliza-o ou não. As alternativas existentes são, em relação ao meio em que o serviço será provido no ambiente, como por exemplo, a relação do sistema operacional ou o software que irá prove-lo.

Como o tema tratado refere-se a pequenas redes, temos diversas situações (exemplos) em que não é adotado o DHCP.

Ao descrever as vantagens, toma-se por premissa um ambiente em que os endereços de IP são atribuídos manualmente em cada maquina, porém a quantidade de vantagens pode aumentar consideravelmente (ou diminuir) através de diversos fatores, como por exemplo, o tipo de programa que irá prover o serviço na rede, pois cada um possui particularidades que agregam recursos secundários dando novas possibilidades e ferramentas para o administrador da rede, isto, associado também com o tipo de hardware em que se encontrará o serviço de DHCP.



### Algumas vantagens


Simplifica a administração da rede, porque alivia a tarefa de configuração do TCP/IP por parte dos administradores da rede.

Ajuda a controlar/monitorar a utilização do espaço de endereçamento IP.

Qualquer tarefa que exigir a troca do modo de endereçamento em uma rede poderá ser facilmente concluída/realizada com a utilização do DHCP. Por exemplo, uma maquina que recebia IP Dinâmicos, agora necessita que seu IP seja estático, pois começou a prover um serviço na rede.

Uma mudança nos endereços IP dos servidores DNS de uma rede, por exemplo, não precisa ser trocada em cada estação de trabalho. Bastaria configurar o servidor DHCP, que este se encarrega de configurar as estações de trabalho, informando-os sobre os novos endereços.


Não representa custos adicionais, pois não é necessário dedicar um servidor somente para serviço DHCP.

Impossibilita erros de conflito na rede (endereços duplicados), entre outros.

Multiplataforma, o servidor DHCP suporta qualquer tipo de maquina, desde que a maquina tenha suporte ao DHCP, obviamente. Ou seja, podemos ter em uma rede Diversos tipos de maquinas e sistemas operacionais (Windows, Linux, Unix, BSD, MAC OS, entre outros...)

Obviamente a maior vantagem de todas é a questão da "previsão de futuro" ou "previsão de crescimento", adote como exemplo o seguinte ambiente:
Uma empresa deseja crescer, a rede de informática juntamente com suas ferramentas e recursos é agregada ao negocio da empresa, proporcionando um crescimento MUTUO, ou seja, se a tecnologia da informação será utilizada para auxiliar um crescimento dentro da empresa, portanto o setor de TI também irá crescer.

Em curto ou médio prazo neste ambiente descrito, haverá uma rede proporcional à empresa, porém com os responsáveis pelo setor de "TI" estarão se afogando em configurações manuais em cada maquina, e perdendo-se em vários modos de tentar "documentar" a rede, rastreá-la, identificar cada host e gerencia-los.

Ainda no mesmo exemplo, cada profissional irá documentar a rede de modo diferente, caso este profissional seja trocado, quem assumir seu posto na empresa terá que identificar todas as maquinas novamente.

Outra situação que pode ser adotada como ambiente é: devido ao crescimento rápido, além do responsável pela rede ter que configurar os novos equipamentos, ele (ou o setor de TI) concluem que se deve mudar todos os IP's da rede, como por exemplo, a rede era classe C, e a empresa quer trocar para Classe B ou A de endereçamento de Rede, pois classe A e B permitem muito mais Hosts conectados à rede do que os IP's de classe C. Ou então, uma segunda rede de computadores será adicionada, agregada a atual rede da empresa.

Neste mesmo ambiente, a politica de segurança se tornará falha, será necessária a contratação de mais profissionais, pois a cargas de tarefas também será grande. O que impede um usuário mal intencionado ou desinformado que troque seu endereço de IP? 

Há diversos exemplos e situações em que a configuração manual de endereços em cada maquina numa rede pode trazer consequências negativas, prejudicando assim, toda a corporação e seus profissionais (podemos dizer "usuários").

A simples adoção de um serviço de DHCP pode contornar todos estes ambientes. Pois independente do crescimento da empresa e do aumento da rede de computadores, o DHCP fará todo o trabalho, quase sozinho, dando ao administrador da rede, a tarefa de se encarregar de politicas se segurança e outros fatores e serviços em sua rede.


### As possíveis desvantagens

Listar as desvantagens do uso do serviço de DHCP é uma tarefa difícil, não há outro serviço "concorrente" ao DHCP (a não ser a configuração manual), é possível a identificação de algumas desvantagens, porém elas não estão relacionadas ao DHCP, mas sim, algum fator secundário que afeta diretamente este serviço e pode provocar contratempos, ou seja, problemas.



### Algumas desvantagens

A adoção da alocação dinâmica para atribuir endereços de IP em TODA a rede não é conveniente, maquinas como roteadores e servidores são referenciados pelos seus endereços de IP, podemos fazer uma simples pergunta para entender: "Como vou achar alguém (servidor/serviço/roteador) que muda de nome (o IP) constantemente?", ou seja, mesmo utilizando a alocação dinâmica em toda a rede, algumas máquinas deverão possuir IP estático (fixo).

Conclui-se, portanto que, o servidor de DHCP não será o "salvador da pátria" para o administrador da rede, mesmo com ele, haverá maquinas para utilizar outro tipo de atribuição de endereço de IP.

Redes em que a politica de segurança consiste em permitir ou negar acesso/serviço através da identificação do endereço de IP ou nome da maquina cliente terá uma desestruturação completa da segurança, uma vez que o IP não irá pertencer mais a uma determinada maquina.

Em redes que esta politica de segurança é aplicada, o uso da alocação dinâmica deverá ser restringido a um grupo especifico de maquinas que se enquadram na mesma politica de segurança, acesso e/ou serviço.

Neste exemplo, é mais fácil e menos traumático utilizar vários métodos de atribuição de IP do que repensar e mudar toda uma politica de segurança dentro da rede.

O uso do DHCP com alocação dinâmica pode comprometer seriamente a segurança de uma rede cujos pontos de acesso não são controlados, ou são utilizados por usuários "não confiáveis". As máquinas clientes buscam o serviço de DHCP na rede através de Broadcast e o servidor irá responder, mas quem pode garantir que este servidor que respondeu é o legitimo da rede? O DHCP é construído sobre o protocolo UDP, que é um protocolo inseguro, herdando, portanto, as suas falhas de segurança.

Nos exemplos de possíveis "desvantagens" descritas acima, não estão relacionadas diretamente ao serviço de DHCP, mas sim a outros fatores, como a falta de conhecimento do responsável pela implantação do serviço de DHCP e também do administrador da rede e pontos falhos na politica de segurança.

Como em qualquer processo ou serviço e a implantação dos mesmos, é preciso que a configuração dele seja apropriada, analisada e desenvolvida para a rede em questão, também é extremamente importante, obviamente, o conhecimento do serviço, suas possibilidades, recursos e falhas pelo administrador da rede.


# APIPA

Atualmente, numa rede em que está implantado o serviço de DHCP e, caso a conexão com o servidor esteja inacessível e não haja uma configuração manual da rede, a maioria dos sistemas operacionais atuais utiliza um sistema chamado APIPA (Automatic Private IP Address) Endereço Automático de Endereço IP, em que o autor Carlos E. Morimoto no livro “Redes-guia prático” (página 139) descreve seu funcionamento:

> “Com o APIPA, o host utiliza im endereço aleatório dentro da faixa de IP’s 169.254.x.x com máscara 255.255.0.0, uma faixa reservada não-roteavel, definida pela IANA em 2001. Este endereço temporário permite que o host comunique-se com outras maquinas da rede que também estão fazendo uso do APIPA, mas não permite que haja um acesso a Web, redes externas ou participe da rde local até que a rede seja realmente configurada, manualmente ou com o servidor dhcp sendo reinserido novamente a rede .”


# Diagnóstico

Já foi descrito anteriormente sobre o que é o DHCP, seu surgimento, características, benefícios e possíveis desvantagens. Apresentaram-se diversas situações e exemplos em que devido ao crescimento da rede é necessária a implantação deste tipo de serviço.

De acordo com estudo realizado pelo Serviço de Apoio às Micro e Pequenas Empresas de São Paulo SEBRAE-SP em 2008 chamados "As Tecnologias de Informação e Comunicação (TICs) nas MPEs brasileiras" disponível em http://www.sebraesp.com.br/conhecendo_mpe/estudos_tematicos/tecnologia_informacao
Houve um crescimento de mais de 450% entre 1998 e 2008 no uso e adoção de microcomputadores dentro das micro e pequenas empresas, para esta conclusão foi pesquisada 4.004 empresas e entre elas, 60% atribuem "grande importância" ao uso de microcomputador no negócio, e 51% à internet.

O computador foi citado como o mais importante recurso tecnológico. Porém o uso é de forma limitada: apenas para armazenar informações sobre clientes e redigir documentos, controlar finanças e compartilhamento de impressora e da conexão com a internet. 66% das empresas utilizam softwares como banco de dados, ERP's e outros.

Uma péssima constatação e de que as redes de computadores ao implantadas nestas empresas sem haver um planejamento, as ferramentas e os recursos são encaixados na medida em que a empresa cresce. Haverá um momento em que este tipo de "crescimento" não será suportado, fazendo com que sejam obrigados a pausar e re-estruturar o setor de TI, isto é apenas uma questão de tempo.

Portanto, há um ambiente em que o crescimento desordenado do setor de Informática impactará negativamente sobre toda a corporação. Planejamento e estruturação são os pontos mais importantes para conter e suportar toda a demanda e necessidade que a rede (TI) e a empresa irão desenvolver.

Garantindo assim, um crescimento saudável e todos estarão introduzidos num ambiente em que a informática é uma ferramenta que agrega importância e qualidade no desenvolvimento da corporação e estando totalmente preparada para mudanças, suprindo todas as necessidades no mesmo ritmo de crescimento da empresa.

A implantação do serviço de DHCP dentro da rede é um dos primeiros passos que deve ser adotado no planejamento, permitindo a ótima administração da rede de forma simples e já obtendo uma estrutura para suprir as futuras necessidades, independentemente da taxa de crescimento da rede e/ou da corporação.

## Endereços de IP

Cada host ou estação conectada a rede recebe um endereço lógico IP (Internet Protocol), em oposição, cada equipamento possui um endereço MAC, que é físico.
Cada endereço IP  exclusivo para cada interface na rede, que se comunica com os outros equipamentos pelo protocolo TCP/IP.

### IPv4
O IPv4 é o "formato" (podemos dizer "estrutura"), é a versão utilizada atualmente para se construir o endereço de IP. Ele é representado por um número de 32 bits, arranjado sob o formato de 4 octetos, ou seja, quatro conjuntos de oito números binários (1 e 0), como por exemplo: 11100010.011011100.01110001.11001000.

Para facilitar o endereçamento IP e a sua administração/gerenciamento, além da divisão em 4 grupos, os números binários são convertidos em decimais, ficando o exemplo acima assim: 226.220.113.200.

Há 5 classes de IP, classificadas como A B C D e E. As classes D e E são reservadas para multicast. Para endereçamento, utilizam-se as classes A, B ou C.

Porém, dentro dessas classes há os chamados IP's Públicos, que são destinados para a internet, permitindo a conexão com a mesma (servidores públicos, paginas da web, etc.).

Para o endereçamento de uma rede, utilizam-se os denominados IP's Privados, que não são roteáveis, sendo, portanto, utilizados em redes privadas, onde não há conexão com a internet, quando ela existe, está sendo compartilhada através de um equipamento (servidor, roteador,...) que possui 2 interfaces, uma com o IP publico (internet) e a outra com o IP privado (rede), fazendo assim um roteamento, ou seja, a comunicação entre 2 ou mais redes, isto é feito por um servidor que irá prover a conexão, um roteador, gateway, firewall, proxy, etc.

Os endereços privados de IP (IPv4) estão definidos na RFC 1971.
A diferença entre as classes A B C, basicamente é a quantidade de rede que cada um possibilita juntamente com a quantidade de hosts em cada uma destas redes suporta (quanto maior um, menor o outro número de possibilidades.).

O objetivo deste trabalho não é a descrição dos IP's, então a análise dos mesmos, a função de cada bit, a estrutura de cada classe de endereços fica como proposta de tema para um futuro estudo do protocolo IP e endereços.


### IPv6
Atualmente, os endereços de IP públicos (usados para a internet), já se tornaram escassos, a previsão era de que até o ano de 2013 os chamados ISP's, provedores de serviços de internet, já estariam prontos e com o IPv6 funcional, ou seja, trabalhando! Nem todos estão assim hoje, os que ainda não migraram, sofrem com a reutilização destes endereços e fazendo malabarismos para conseguir trabalhar.

Para se tornar um ASN (Sistema Autônomo de Internet) hoje, não são mais dados endereços IPv4.

Entre algumas particularidades do IPv6 há:

A autoconfiguração da rede (ou seja, fim do DHCP), o fim das colisões com endereços privados, o NAT torna-se opcional não mais uma necessidade, entre várias outras melhorias...

O Linux possui suporte ao IPv6 desde os Kernels 2.1.X, Windows (desde o Server 2003 e Xp), MAC OS X, e a maioria dos equipamentos de rede (switches, roteadores, interfaces, etc...), incluindo câmeras, celulares, PDA's, entre outros.

De acordo com dados do IANA, órgão responsável pela distribuição dos blocos de endereços IP, em http://www.iana.org/,  mostra que o número de provedores de serviço de internet que oferecem IPv6 é crescente, nos EUA a adoção cresce rapidamente, eles foram os primeiros a sentir a necessidade de mais endereços públicos de IP (possuem 60% dos endereços IPv4 existentes), o Japão e a União Europeia lideram a lista de adoção do IPv6 para endereços Públicos.

Atualmente, apenas resta 9 blocos de IP disponíveis para futuras utilizações, a partir do começo de 2012 qualquer novo link, enlace ou conexão Internet, quer seja com IP válido ou inválido, quer seja com IP fixo ou dinâmico, vai ser obrigatoriamente IPv6.
Porém os responsáveis pelos registros de endereços (AfriNIC, APNIC, ARIN, LACNIC e RIPE NCC) já estão tomando ações para a conscientização e substituição dos endereços em formato IPv4.

Dentro das redes privadas, não há necessidade da adoção de IPv6 e de um servidor para "DHCPv6", ainda não trás nenhuma vantagem e/ou desvantagem.

O IPv4 não é compatível com IPv6, mas existe diversas formas de "trabalho em conjunto", como por exemplo a configuração de túnel de IPv4 para IPv6 na rede.
Por um bom tempo, provavelmente, por um longo período de tempo, teremos redes privadas IPv4 conectadas a internet provida de um gateway/proxy/roteador com IPv6.

Todo o trabalho desenvolvido, ferramentas e conceitos apresentados, na implementação, configuração e uso do servidor de DHCP não leva em consideração, nem adota o IPv6 como ambiente em uso na rede privada, muito menos como endereço publico para a conexão com a internet.




# Terminologia e seus significados

Durante a implantação do serviço, haverá diversos termos que serão aplicados na descrição da configuração, independente do "formato" escolhido. Abaixo, apresento alguns destes termos que serão mencionados juntamente com seus significados.

**Host / cliente** - É o "individuo" que irá solicitar o endereçamento, pode ser qualquer interface que esteja conectada a rede, como um microcomputador, notebook, impressora, etc...

**Escopo** - Intervalo consecutivo dos possíveis endereços IP para a rede. É o método-base em que o servidor irá aplicar todas as  configurações.

**Superescopo** - Utilizado para a distribuição de endereços e não para configurações, ele é um conjunto administrativo que pode ser usado para criar várias sub-redes IP lógicas (grupos) dentro da mesma sub-rede

**Intervalo de exclusão** - Intervalo de endereços IP que o Servidor não poderá disponibilizar para as interfaces que irão requisitar um endereçamento.

**Concessão ou “lease time”** - Período de tempo em que o servidor atribuirá para um cliente permanecer com o endereçamento, quando este tempo ainda é válido e o cliente está "usando-o", chama-se de Concessão Ativa.

**Reserva** - Concessão de endereços permanente. Ou seja, quando é configurado para que um ou mais hosts específicos sempre utilizarem o mesmo endereço IP.





# Implantação do DHCP

Não há um "formato" que é intitulado como a "melhor maneira de aplicar o servidor de DHCP" em uma rede. Existe sim, vários modos de implantação, cada um com suas particularidades, pontos positivos e negativos.

Não se deve procurar uma "solução pronta", o administrador da rede deve analisar sua rede, somar com a projeção de crescimento, estrutura atual, futuras mudanças, aplicação de seu uso, entre outros fatores, e com isto irá traçar a melhor solução para prover este serviço em sua rede.

Para a implantação do DHCP, pode-se utilizar diversos formatos, entre eles há:


- Utilizando um roteador para prover o serviço
- Utilizando a plataforma MS Windows Server
- Utilizando servidor Linux

# Utilizando Roteadores para prover DHCP

Atualmente, devido a popularização da banda larga em ambiente doméstico, há no mercado um número enorme de fabricantes de roteadores e switches que tem como público consumidor os usuários domésticos (home) e pequenas redes (small offices), estes equipamentos englobam diversas ferramentas e possibilidades, permitindo ao responsável pela rede, atribuir quase todas as tarefas para o funcionamento da rede a estes equipamentos.

Com esses dispositivos é possível compartilhar a conexão com a Internet entre todos os computadores, além de arquivos e impressoras, os roteadores também funcionam como firewall baseado em hardware, além de disponibilizar portas como um switch (tanto ethernet quanto wireless), e também podem prover o servidor de DHCP distribuindo endereços paras todos os hosts da rede.


Custos e Configuração
A quantidade de tarefas que ele pode executar na rede comparado ao seu custo, torna-o uma das soluções mais adotadas para DHCP em uma pequena rede.

O público alvo deste mercado é o usuário domestico e pequenas empresas, sua configuração é fácil, a mão-de-obra e manutenção possuem baixíssimos custos. Um dos modelos mais baratos encontrado é o “D-Link DIR-100”, com valores a partir de R$ 46,66.


![fig 1.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780534155/2hj2ezpSp.jpg)


No mercado, além da D-Link, encontra-se outras fabricantes com produtos para este público consumidor, como a TPLink, IntelBras, OvisLink, Pacific Network, Tenda, HP (3com), e também uma das maiores e mais conceituada fabricante de equipamentos, a Cisco, com sua linha denominada LinkSys, desenvolvida para este tipo de mercado.

Diversos destes aparelhos possuem Sistema Operacional Embarcado, com Linux, mas independentemente, todos esses dispositivos são configurados a partir de qualquer host conectado a rede, através do navegador web deste host.

Apenas basta digitar no navegador, o endereço IP do roteador para ter acesso às suas opções de configuração. Esta informação está escrita no manual de cada um, mas geralmente endereços IP utilizado são 192.168.0.1, 192.168.1.1 ou 10.0.0.1. Ao acessar, será disponibilizado uma interface gráfica extremamente simples para configurar.

Além de determinar o bloco de endereços IP que será utilizado, há outras configurações, como firewall, DNS, tipo de conexão. 


![fig 2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780541924/N7bJ6XuAG.png)


Para a atribuição do endereço de IP estes equipamentos dispõem dos 3 modos de alocação: Automática, Dinâmica e Manual:




![fig 3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780553413/MXWytLdUh.png)


na figura acima há a alocação manual, onde deve-se indicar um endereço MAC de um host, e o endereço de IP que este host sempre deverá receber quando fizer a requisição na rede.

O problema que será enfrentado futuramente é que este tipo de solução não irá suportar o crescimento da rede, tanto em número de hosts, quanto ao surgimento de novos servidores, principalmente quando as configurações de firewall se tornam complexas, como por exemplo, a introdução de servidores públicos, criando assim uma DMZ (zona desmilitarizada), entre outras situações.




# DHCP em Roteadores Cisco


![Cisco (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780580532/1zUl-VetI.png)

Uma alternativa seria a implantação de roteadores com maior capacidade e desempenho, consequentemente maior preço, mas não traria estes tipos de gargalos futuramente. Pode-se recorrer aos equipamentos Cisco, ou de sua divisão LinkSys.

A configuração é mais complexa e a implantação é mais cara também.
Na configuração, utiliza-se um cabo para conectar a porta console do equipamento, e as configurações são feitas em modo terminal, ou seja, não há interface gráfica.

Configuração básica do Roteador Cisco para prover DHCP:

Após atribuir um endereço a interface ethernet em que a rede está conectada, com os comandos:


```
Router(config)# interface fastethernet 0/0
Router(config-if)# ip address 192.168.0.1 255.255.255.0
Router(config-if)# no shutdown
``` 

Fica determinado que, a rede (hosts, switches..) conecta-se ao roteador através de sua interface ethernet chamada 0/0, é que seu IP é 192.168.0.1, com a mascara 255.255.255.0. Agora, é necessário ativar o serviço de DHCP nesta interface do roteador, com o comando:

```
Router(config)# service dhcp
```

Com o serviço ativado, agora deve ser informado o bloco de endereços que o equipamento cisco irá utilizar para atribuir os endereçamentos quando um host solicitar, como está sendo usada a rede 192.168.0.0, insere os comandos:


```
Router(config)# ip dhcp pool redelocal
Router(dhcp-config)# network 192.168.0.0 255.255.255.0
Router(dhcp-config)# default-router 192.168.0.1
Router(dhcp-config)# exit
```


Após isto, o roteador já está pronto para atribuir endereços dinâmicos as interfaces que fizerem a requisição na rede.



Aprofundando-se no sistema IOS presente nos equipamentos Cisco e lembrando que cada série de equipamentos possui suas particularidades e características, há diversas possibilidades, como por exemplo, configurar um intervalo de endereços que não será utilizado pelo roteador em suas atribuições, estes endereços podem ser atribuídos manualmente para hosts que necessitam de IP estático.

Para isso, utiliza-se o comando:

```
Router(config)# ip dhcp excluded-address 192.168.0.10 192.168.0.20
```

Para obter um status com o objetivo de monitorar o serviço e gerenciá-lo, ou seja, uma forma de troubleshooting há alguns comandos que podem ser executados no roteador:



> show ip dhcp binding

Lista os IPs já fornecidos para as estações de trabalho.

> show ip dhcp conflict

Lista eventuais conflitos de endereços IP.

> show ip dhcp database

Lista o DHCP database.


> show ip dhcp pool

Lista todo o conteúdo dos pools configurados.


> show ip dhcp relay information trusted-sources

Lista as informações sobre o relay DHCP.


> show ip dhcp server statistics

Mostra as estatísticas do tráfego DHCP.


> debug ip dhcp server events

mostrar os "empréstimos" e "expirations" dos endereços IP.


> debug ip dhcp server linkage

diagnostica eventos em situações onde existem relações entre dois ou mais servidores DHCP


> debug ip dhcp server packet

Mostra o tráfego DHCP em tempo real.

Obviamente existem muito mais opções de configuração em roteadores Cisco, o que exige um estudo profundo e a determinação de um modelo de equipamento, já que ocorre uma variação de possibilidades e ferramentas disponíveis em cada modelo de roteador Cisco.


# DHCP com Microsoft Windows Server



Para prover o serviço dhcp utilizados a plataforma Windows pode-se utilizar qualquer versão a partir da "Windows Server 2000".
Caso a rede já possua um servidor Windows, esta poderá ser uma das melhores opções para  a adoção do provedor de dhcp.

As configurações no Windows Server são fáceis de serem aplicadas, as politicas de endereços são facilmente configuráveis e a interface gráfica é muito intuitiva, além de utilizar um assistente para a configuração, que faz com que o administrador configure passo-a-passo, cada opção, e todas elas veem com texto explicativo e exemplos de aplicação.

Este assistente é presente não só na configuração do servidor DHCP, mas em quase todos os recursos e serviços oferecido pela plataforma de servidores Windows, desde a versão 2000 até a mais recente, 2008.

Para aplicar o servidor, podemos utilizar o Windows Server 2000, 2003 (o mais utilizado atualmente) e 2008, que hoje está sendo adotado em substituição ao 2003 em redes que estão sendo atualizadas/migradas, e que é adotado  em novas redes.

O Windows Server 2008 é a plataforma vendida atualmente no mercado, é interessante a aplicação do servidor DHCP por meio do Windows Server, caso a pequena rede, já esteja operando e com este sistema operacional presente.

A politica de licenças da Microsoft é confusa e complexa, onde o valor da compra deste sistema operacional vai depender de sua finalidade do uso, quantidades de usuários e máquinas conectadas a rede utilizando os serviços, se haverá acesso remoto, quantidade de serviços que o sistema operacional irá prover, entre outros fatores.

Com isto, seu valor final fica em torno de US$ 1.029,00 (Windows Server 2008 R2 Standard, incluso 5 CAL’s, licenças de usuário para acesso), US$ 3.999,00 (Windows Server R2 Enterprise, incluso 25 CAL's, licenças de usuário para acesso).


Caso necessite, posteriormente, que o Windows server passe a fornecer outros serviços, novas licenças terão que ser adquiridas, que podem ser licenças de um novo Windows Server, como upgrade da versão, ou licenças adicionais para um serviço especifico, como por exemplo a  "Licença de servidor para Usuários Externos acessando Serviços de Área de Trabalho Remota do Windows" (External Connector License de Serviços de Área de Trabalho Remota do Windows Server 2008 R2) que custa US$ 7.999,00.

Estes valores são fornecidos pela página na internet da Microsoft, dando uma estimativa final de preço, que pode variar (geralmente para mais), os dados estão presentes no seguinte endereço:
http://www.microsoft.com/windowsserver2008/pt/br/pricing.aspx


Não é vantajoso procurar licenças do Windows Server 2003, seu preço pode variar 30% a menos, além de que em breve, não haverá msis suporte nem atualização. Ou seja, todas as corporações que utilizam o Windows Server 2003 ou 2000, independente de seu porte, terá que migrar a rede para o Windows Server 2008, é apenas questão de tempo.



Habilitando o serviço
A interface para ativar os serviços que o Windows irá prover é relativamente semelhante, independente da versão do sistema e do serviço desejado.

Windows Server 2000
Clicando em:
Iniciar > Configurações > Painel de Controle > Adicionar/remover programas > Adicionar/remover componentes do Windows.


Nesta janela, serão mostrados os recursos do Windows, com a possibilidade de ativar ou desativá-los.

No assistente de componentes do Windows, clica em serviços de rede, e depois em detalhes. Na caixa "Serviços de rede", seleciona a caixa "Protocolo de configuração dinâmica de hosts (DHCP)" e depois basta clicar em OK. O serviço será ativado, e a janela para sua configuração será aberta.

Windows Server 2003
Para habilitar o serviço no Server 2003, clica em:
iniciar > programas > ferramentas administrativas > Assistente para configurar o Servidor.


Seleciona a caixa "Servidor DHCP" e depois basta clicar em Avançar. O serviço será ativado, e a janela para sua configuração será aberta.


## Configuração

Os passos para a configuração descritos abaixo partem a partir deste ponto, a configuração no Windows Server 2000, 2003 e 2008 são semelhantes, a descrição foi desenvolvida tendo como base o Windows Server Enterprise 2003, além das figuras.

Ao chegar à janela do assistente para configurar o servidor, independente da versão do Windows, este assistente lista diversos serviços, que podem ser ativados e/ou removidos (caso já estejam ativos).




![fig 4.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780668508/skrXaKKdD.jpg)

Basta clicar em “Servidor DHCP” e depois em Avançar, o Windows server, nesta etapa irá carregar os arquivos necessários para que ele possa prover e executar este serviço, além das configurações iniciais para o sistema passar a fornecer a administração e gerenciamento do mesmo.
Veja nas Screenshots abaixo:



![fig 5.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780683485/lwHn5ANkQ.jpg)


![fig 6.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780686707/kQPtMZeCO.jpg)


![fig 7.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780690968/kCcSdb5v8.jpg)



![fig 8.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780694940/F15Rpgqx9.jpg)



Após a conclusão desta etapa, não costuma demorar, logo em seguida já irá ser aberto a janela para a criação dos escopos para o servidor.


![fig 9.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780702258/MgpI7mNJ9.jpg)





Ao clicar em Avançar, a primeira configuração a ser definida, é o range, ou pool de endereços, ou seja, o endereço inicial e o endereço final, os endereços de IP que estarão dentro deste conjunto serão os endereços que o servidor vai disponibilizar para as maquinas clientes, além dos endereços inicial e final, ele solicita o comprimento e a máscara.


![fig 10.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780710474/SOdEVwjQl.jpg)

Na configuração adotada aqui, utilizou-se os endereços 192.168.1.1 até 192.168.1.200.
A janela seguinte é opcional, o administrador pode inserir alguns endereços que serão exclusos do pool disponível para os clientes, neste caso, foi adicionado endereços com final múltiplo de 50 (50, 100, 150, 200).

![fig 11.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780720796/Ls9VWV1Nc.jpg)

O próximo passo será determinar o tempo de concessão, ou lease time, o tempo que cada cliente irá permanecer com o endereço. Não há tempo ideal, para determina-lo, deve-se analisar a função que os clientes exercem na rede e o impacto que irá causar caso a troca de endereços seja constante ou não.




![fig 12.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780736655/HJCgm0cYL.jpg)





Na janela seguinte, é definido se este escopo recém-configurado deve ser ativado (entrar em funcionamento) agora, ou num horário especifico. Ao clicar em avançar, será dada a janela de sucesso, avisando que o servidor passa agora a fornecer o serviço de DHCP.


## Gerenciamento

A tela para o gerenciamento/administração do serviço DHCP é bem simples e intuitiva, é visualizado o domínio, em formato de “árvore”, após ele, o escopo, ou os vários escopos (caso seja inserido mais do que um), além de opções para visualizar as concessões ativas (endereços sendo utilizados no momento), reservas, opções de escopo, entre outros.


![fig 13.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780749473/YYBHmI5-u.jpg)



# DHCP no GNU/Linux

O Linux é um sistema operacional multiplataforma baseado no UNIX, desenvolvido por Linus Torvalds (Depto. de Ciência da Computação da Universidade de Helsinki, Finlândia, 1991) a partir de um sistema chamado MINIX, cujo desenvolvedor foi Andrew S. Tanenbaum, muito conceituado também pelo seu livro “Rede de computadores”.

O Linux é um software livre, permitindo que qualquer pessoa utilize, estude, altere, e distribua diferentes versões do seu código-fonte.

O termo GNU/Linux é empregado para caracterizar qualquer sistema operacional do tipo Unix que utiliza o núcleo Linux, e programas de sistema GNU. Pelo fato de existirem muito poucos exemplos de sistemas com núcleo Linux sem programas GNU, os termos GNU/Linux e Linux são utilizados como sinônimos na maioria das vezes.

Além de estudantes, voluntários e programadores independentes, grandes empresas também contribuem para o desenvolvimento do Linux, como a IBM, Oracle Hewlett-Packard, Novell, entre muitas outras, tornando assim o Linux no principal sistema operacional para servidores.
Atualmente, 80% dos Hostings mais confiáveis utilizam o Linux como sistema operacional de servidores web.

O sistema Linux é multiplataforma, pois funciona no maior número de arquiteturas computacionais possíveis. O Linux é utilizado como sistema operacional em diversos tipos de equipamentos, desde supercomputadores até celulares.

O Kernel de um sistema operacional representa a camada mais baixa de interface com o Hardware, é o gerenciador de recursos de todo o sistema. No Kernel são definidas as funções para operação com periféricos (mouse, teclado, discos, impressoras, etc.), gerenciamento de memória. Ou seja, o Kernel é um conjunto de programas que fornece aos aplicativos uma interface para manipulação dos usuários do sistema.
Atualmente, o sistema operacional GNU/Linux é distribuido por organizações do mundo todo, além de inúmeros programadores e entusiastas que auxiliam em seu desenvolvimento de alguma forma, como traduções, programação, softwares para diversas funções e aplicações, etc.


As mais famosas companhias que desenvolvem e distribuem o GNU/Linux, oferecendo suporte e auxilio à corporações que o adquirem são: Red Hat, SuSE, Mandriva (resultado da fusão entre Mandrake e Conectiva). Outras distros são distribuídas e mantidas por comunidades através da internet, como no caso do Gentoo, Debian, Slackware, Fedora.

Segundo o site http://distrowatch.com existe atualmente mais de 500 distribuições diferentes, cada uma desenvolvida para uma área especifica, tipos de usuários e destinada a plataformas e aplicações distintas.

Ubuntu, Fedora, Mint, OpenSUSE, Debian, PCLinuxOS, Mandriva, Arch, Slackware, FreeBSD, CentOS, Gentoo, KNOPPIX e Red Hat são as distribuições GNU/Linux presentes no ranking das 100 mais famosas e acessadas dos últimos 12 meses.





## Softwares Para Prover DHCP no Linux

Diferentemente do Windows Server, cujo software que irá prover o serviço de DHCP já está incluso no sistema operacional, cabendo ao administrador ativá-lo e configurar, não havendo portanto uma opção de escolha sobre o software, já que não é o sistema operacional que prove o serviço, e sim um software nele instalado, no GNU/Linux existe esta capacidade, pois seu Kernel não é fechado e nenhum software exerce um monopólio sobre algum serviço.

Dentre os programas que podem ser instalados nas distribuições GNU/Linux e prover o servidor de DHCP, há o BusyBox, Dibbler, ISC DHCP, e mais alguns.

BusyBox DHCP server ou também conhecido como udhcpd (servidor) e udhcpc (cliente) é famoso por ser um pequeno servidor/cliente, geralmente adotado em sistemas embarcados ou em situações onde as memórias ROM e RAM são extremamente escassas, e que o sistema linux é modificado para ocupar, por exemplo 20MB de espaço no disco e 8 ou 10MB na memória RAM.

Wide-DHCPv6 originalmente desenvolvido no projeto KAME, pela universidade Keio do Japão (Minato, Tokyo), é um servidor dhcpv6, ou seja, somente IPv6, para os sistemas operacionais linux e BSD, ele foi concluído e parado em 2006.

Há também uma modificação do Wide-dhcpv6, chamada Dibbler que prove o DHCPv6 server e prove endereçamento para linux (kernel 2.4 e 2.6), Windows (A partir do XP, 2003 e superiores), Windows NT4 e 2000 (mas é experimental) e MAC OS X.Porém o Dibbler está com seu desenvolvimento parado.




# ISC-DHCP

ISC DHCP é o mais conhecido e utilizado, desenvolvido pela “Internet Systems Consortium” uma organização sem fins lucrativos que desenvolve e distribui softwares no formato “Open Source” além de prestar suporte, prezando pela mesma qualidade das maiores empresas de softwares comerciais. Além do DHCP mais utilizado, eles também desenvolvem o sistema de DNS mais adotado, conhecido como BIND, entre outros projetos também desenvolvidos e/ou distribuídos pela ISC estão o NTP, INN, IRRToolSet, libbind, OpenReg, operam também o “F-root”, que é um dos 13 roteadores DNS que mantém a internet funcionando em todo o mundo.

ISC-DHCP é uma coleção do software que executa todos os aspectos do DHCP (protocolo de configuração de host dinâmico).

Inclui um servidor DHCP, que receba pedidos e responde, um cliente DHCP, que estará no sistema operacional do computador do cliente e que emita pedidos ao servidor, um agente relay DHCP, que passa pedidos do DHCP de uma Rede LAN a outra de modo que não precise de um servidor DHCP em cada LAN.

O servidor de ISC-DHCP responderá a pedidos de todos os clientes que cumprem com os padrões de protocolo definidos na RFC, e o cliente ISC-DHCP pode interagir com qualquer servidor, desde que também cumpra esses padrões. 

Portanto, os componentes do ISC-DHCP não precisam ser utilizados em conjunto, por haver a padronização que é regida pelas RFC's, há a interoperabilidade entre clientes e servidores de todas as soluções para a implantação do DHCP, comercial ou Open Source e de qualquer plataforma e/ou sistema operacional.

O ISC DHCP server foi desenvolvido originalmente por Ted Lemon, que o manteve até o release 3.0, com a versão alpha liberada em março 1999 e sua versão final em janeiro 2003.

Desde 2004, a manutenção e o desenvolvimento preliminar do ISC DHCP, especialmente o suporte ao IPv6 está ao cargo de David Hankins no ISC.

A versão usada do ISC DHCP é 4.2.4.P1 que foi lançada em 24 de Julho de 2012.
Essa versão possui diversas características novas que a versão anterior (4.0.X) não possuía, entre elas o DHCPv6 foi melhorado, suporte a delegação de prefixo, suporte a endereços IA_TA, agente relay DHCPv6 (dhcrelay6).

Todas as características desta versão são encontradas nos arquivos que o acompanham, no documento chamado "Release Notes" encontra-se também o que será implantado na próxima versão,que já está em testes e desenvolvimento.

Um dos recursos que está sendo melhorado é o suporte ao DHCPv6, uma de suas limitações na versão atual, é que apenas tem suporte para os Sistemas Operacionais Solaris, Linux, FreeBSD, NetBSD, e OpenBSD.

O cliente e o servidor somente operam com DHCPv4 ou DHCPv6 por vez, não ao mesmo tempo, para que haja este serviço simultâneo, deve ser executado duas instâncias do serviço.

Uma das fontes de receita e captação de recursos da ISC é a disponibilização de suporte profissional à empresas, consultoria, treinamento a administradores de sistema e em breve um programa de certificação para profissionais.



## Instalação e configuração do ISC-DHCP



A versão mais recente pode ser obtida no endereço:
http://www.isc.org/software/dhcp
Após baixar, basta descompactar e compilar o pacote.

Outra forma de instalar o ISC-DHCP server, é através do repositório da distribuição GNU/Linux que está sendo utilizada. O repositório é basicamente um servidor de arquivos disponível na internet, onde possui diversos programas e pacotes pré-compilados para a distribuição GNU/Linux especifica, a maioria dos repositórios somente são acessíveis através do gerenciador de pacotes presente na distribuição Linux, basta que a maquina em questão esteja conectada com a internet.

 Nas distribuições derivadas do Debian (ubuntu, kurumin, Mint etc.), o pacote correspondente ao servidor DHCP se chama "dhcp3-server", para instalá-lo basta digitar:


```
# apt-get install dhcp3-server
```

No Red Hat ( e distribuições derivadas dele, como o Fedora e CentOS), o pacote se chama simplesmente "dhcp" e para instalá-lo basta digitar:


```
# yum install dhcp
```
Embora o pacote se chame apenas "dhcp", o script referente ao serviço se chama "dhcpd", de forma que os comandos para iniciar, parar o serviço ou reinicia-lo são:


```
# service dhcpd start
# service dhcpd stop
# service dhcpd restart
```

Ou também através dos comandos:
```
# /etc/init.d/dhcp3-server start
# /etc/init.d/dhcp3-server stop
# /etc/init.d/dhcp3-server restart
```


Também pode haver a necessidade de ativá-lo ou desativar manualmente para iniciar junto com o sistema (junto ao “boot”) usando o comando "chkconfig":


```
# chkconfig dhcpd on
# chkconfig dhcpd off
```

O arquivo de configuração é o "dhcpd.conf", nesse será configurado o servidor, e lá estará inserido todas as informações a cerca de seu funcionamento.

Ao instalar o serviço, ele já adiciona o arquivo, porém como não está configurado, o serviço não ficará ativo até que esteja configurado corretamente e dado o comando para iniciar o serviço.

Este arquivo, originalmente possui cerca de 110 linhas, sendo que 90% delas são textos comentados explicando cada item da configuração, como configurar e dando exemplos de configurações.

A localização do arquivo dhcpd.conf no sistema Linux é "/etc/dhcp3/dhcpd.conf", em algumas distribuições pode ser em "/etc/dhcpd.conf".
Após instalar o ISC-DHCP, deve-se determinar, caso haja mais de uma, qual placa de rede o servidor dhcp estará ativo, ou seja, em qua placa ele atenderá as requisições.

Para isto, basta editar o arquivo presente em "/etc/default/dhcp3-server" na maioria das distribuições. Ao editar o arquivo, haverá uma linha apenas escrito “INTERFACES” nela, deverá ser inserido o nome da placa de rede em que deseja que o serviço seja ativado.

INTERFACES=”eth0”

O GNU/Linux identifica as placas de rede como “eth”, seguido do número a partir do 0, caso tenha 3 placas, serão respectivamente, “eth0”, “eth1” e “eth2”.
Em caso de utilização de placa wireless ela pode ser identificada “wlan0”, “ath0” ou “ra0”, por exemplo.
Ao definir em qual placa o servidor DHCP irá atender, deve-se finalmente configurar.

Nesta etapa, há 3 opções:
Utilizar o arquivo dhcpd.conf original, seguindo as instruções e usando a configuração desejada
Criar um novo arquivo dhcpd.conf e adicionar a configuração
Utilizar um aplicativo em modo gráfico para a configuração
Nas 2 primeiras opções, o arquivo dhcpd.conf ficará basicamente semelhante ao seguinte:


```
# /etc/dhcp3/dhcpd.conf
ddns-update-style none;
default-lease-time 1800;
max-lease-time 14400;
authoritative;
subnet 192.168.1.0 netmask 255.255.255.0 {
range 192.168.1.100 192.168.1.250;
option routers 192.168.1.1;
option domain-name-servers 8.8.8.8,8.8.4.4;
option broadcast-address 192.168.1.255;

host Win2003 {
hardware ethernet 09:0F:B0:FF:EA:10;
fixed-address 192.168.1.2;
}
}
```




Com a exceção de que na primeira opção de configuração, entre cada uma dessas linhas haverá textos explicando cada função, para que serve, como funciona e um exemplo de configuração.


default-lease-time 1800;

Nesta linha, é indicado ao servidor, checar a cada 1800 segundos (30 minutos) se a maquina está ativa, ou seja, utilizando o endereço dhcp.


max-lease-time 14400;

Nesta opção, a máquina cliente irá receber o “aluguel” do endereço IP por 14400 segundos (4 horas), ou seja, é o tempo que a maquina vai receber para utilizar o endereço. Depois de decorrido um percentual deste tempo, a máquina cliente vai requisitar a renovação.

Caso a rede possua, por algum motivo, um pool de endereços pequeno, e haja mais maquinas do que IP’s disponíveis, essas duas configurações deverão ter seu tempo reduzido, pois assim, a maquina utiliza o IP apenas pelo tempo em que necessita, e caso seja desligada ou desconectada da rede, o servidor DHCP irá detectar isso em menos tempo, tendo assim o endereço IP, disponível novamente para ser atribuído à outro host.


subnet 192.168.1.0 netmask 255.255.255.0

Essa linha determina a rede, sub-rede e a máscara da rede em que o servidor irá trabalhar, atribuindo os endereços IP.

As linhas de configuração seguintes definirão a rede, o pool de endereços (range), exceções (IP fixo), endereços de servidores de nome de domínio.

range 192.168.1.100 192.168.1.250;

Range, define o pool de endereços que ficarão disponíveis para a atribuição automática de endereços, neste exemplo, os endereços que o servidor irá atribuir ficam no “espaço” entre 192.168.1.100 e 192.168.1.250, ou seja, há 150 endereços disponíveis para os hosts da rede.

Conclui-se portanto, que há 102 endereços que não fazem parte do range, destinados portanto para máquinas que desempenham algum papel importante na rede e precisam de um endereço físico.


option routers 192.168.1.1;

Esta linha informa para as maquinas qual é o endereço de “gateway” ou do roteador, que está na “divisa” entre a rede interna e a internet por exemplo, exercendo a função de firewall ou proxy na maioria dos casos.


option domain-name-servers 8.8.8.8,8.8.4.4;

Aqui, deve ser informado (opcionalmente) os endereços de servidores de resolução de nome de endereço/domínio, ou DNS. Redes pequenas, em sua maioria, não possuem DNS’s próprios, os endereços aqui inseridos são os DNS’s fornecidos pela provedora de internet, pode-se usar qualquer endereço de DNS público, como por exemplo o OpenDNS (208.67.222.222 e 208.67.220.220) ou o DNS do google (8.8.8.8 e 8.8.4.4)


option broadcast-address 192.168.1.255;

Informado opcionalmente, o endereço de broadcast da rede. O endereço com final 255 é reservado para broadcast.

host Win2003 {
hardware ethernet 09:0F:B0:FF:EA:10;
fixed-address 192.168.1.2;
}



O bloco acima é para definir a atribuição manual de IP fixo para uma determinada máquina, inicia-se com o nome da máquina, neste exemplo ela se chama “Win2003”, declarando que se trata de um servidor Microsoft Windows 2003, e que desempenha algum serviço importante na rede, após o nome, indica-se o seu endereço físico de rede (o MAC da placa), e finalmente, o IP que sempre será dado a esta máquina quando ela requisitar.

Para adicionar mais máquinas que receberão este tipo de atribuição, basta inserir um bloco semelhante a este, mudando apenas o nome, o MAC e o IP destinado a ela.


## Monitorando o DHCP Server via terminal




/var/lib/dhcp/dhcpd.leases

É neste arquivo onde se monitora, em tempo real, o que o DHCP Server está fazendo em relação a conseção dos IPs. (Usando o GNU/Linux Debian, pode haver alguma alteração no caminho do arquivo em outras distribuições).


Exemplo de trecho do Arquivo:


```
}
lease 192.168.0.110 {
  starts 4 2012/07/26 15:31:11;
  ends 4 2012/07/26 15:36:13;
  tstp 4 2012/07/26 15:36:13;
  cltt 4 2012/07/26 15:31:11;
  binding state free;
  hardware ethernet 00:26:5a:06:3d:a2;
  uid "\001\000&Z\006=\242";
}
lease 192.168.0.112 {
  starts 5 2012/07/27 19:52:39;
  ends 5 2012/07/27 20:02:39;
  tstp 5 2012/07/27 20:02:39;
  cltt 5 2012/07/27 19:52:39;
  binding state free;
  hardware ethernet 74:a7:22:9d:a7:0e;
}
lease 192.168.0.100 {
  starts 1 2012/07/30 14:14:15;
  ends 1 2012/07/30 14:24:15;
  cltt 1 2012/07/30 14:14:15;
  binding state active;
  next binding state free;
  hardware ethernet 92:b3:43:b5:c3:fb;
  uid "\001\222\263C\265\303\373";
  client-hostname "vmxp";
}
lease 192.168.0.102 {
  starts 1 2012/07/30 14:18:22;
  ends 1 2012/07/30 14:28:22;
  cltt 1 2012/07/30 14:18:22;
  binding state active;
  next binding state free;
  hardware ethernet 42:d9:7e:be:11:f8;
  client-hostname "lamp";
}

``` 

O arquivo /var/lib/dhcp/dhcpd.leases~ contém dados mais antigos.

O arquivo /var/lib/dhcp/dhcpd.leases armazena o banco de dados de aluguel do cliente DHCP. Este arquivo não deve ser modificado manualmente. As informações de aluguel DHCP de cada endereço IP recentemente atribuído são armazenadas automaticamente no banco de dados de aluguel. As informações incluem datas do aluguel e os endereços MAC da placa de interface de rede usada para recuperar o aluguel.

Todos os horários do banco de dados de aluguel estão em GMT (Greenwich Mean Time) e não horário local.

O banco de dados de aluguel é recriado de tempos em tempos para que não fique muito grande. Primeiramente, todos os aluguéis conhecidos são salvos em um banco de dados temporário de aluguel. Então, o arquivo dhcpd.leases é renomeado paradhcpd.leases~, e o banco de dados temporário é salvo como dhcpd.leases.


No caso do cliente DHCP, o arquivo chama-se /var/lib/dhcp/dhclient.leases
Num dos servidores usado para criar este guia ele tanto é um servidor DHCP quanto um cliente, pois também está funcionando como roteador/firewall/proxy.
Exemplo de trecho do Arquivo:


```
lease {
  interface "eth1";
  fixed-address 187.22.177.60;
  option subnet-mask 255.255.252.0;
  option dhcp-lease-time 10800;
  option routers 187.22.176.1;
  option dhcp-message-type 5;
  option dhcp-server-identifier 201.46.240.45;
  option domain-name-servers 201.46.240.40,201.46.240.45;
  option dhcp-renewal-time 5400;
  option dhcp-rebinding-time 9450;
  renew 1 2012/05/14 13:02:34;
  rebind 1 2012/05/14 14:15:36;
  expire 1 2012/05/14 14:38:06;
}
```

## Interface gráfica para gerenciamento/monitoramento do servidor

Criar, gerenciar, realizar alterações e manutenções no arquivo de configuração do DHCP via terminal não é problema algum para um administrador de rede/servidores Linux. Porém quanto maior as particularidades que possuir em sua rede, quanto mais rápido devem ser realizadas as mudanças, relatórios, documentação ou outras variedades de razões podem ser ótimos motivos para utilização de interfaces gráficas de gerenciamento.

As interfaces gráficas para gerenciamento de servidores no GNU/Linux são extremamente intuitivas e muito avançadas, além do excelente grau de maturidade destas ferramentas. Muitos administradores possuem resistência a adoção destas ferramentas em sistemas Linux, mas esta visão é ultrapassada e não há argumentos que impeça seu uso, não há bons motivos para não usar ;-)

Estas interfaces possibilitam gerenciar todos os serviços implantados num servidor (banco de dados, servidor web, dhcp, dns, firewall, ldap, controlador de domínio, compartilhamento de arquivos, etc.. ), além de realizar configurações do S.O., monitoramento, relatórios e outras possibilidades.

Podemos dividir as interfaces gráficas para gerenciamento de servidores em 2 grupos básicos: Aplicação e Interface Web




### "APLICAÇÃO"


Aplicativo instalado no Servidor (quando o mesmo possui ambiente gráfico, como Gnome, KDE, XFCE, LXDE, ...). Os Aplicativos que permitem o gerenciamento/configuração dos serviços implantados no servidor são acessíveis através do menu do Desktop.


Vantagem: Os paranoicos não precisam esquentar a cabeça sobre a transmissão de dados via rede ou algum sniffer.


Desvantagem: Precisa acessar o ambiente gráfico do servidor, ou seja, ter um teclado+mouse+monitor em seu servidor ou configurar algum acesso remoto como VNC por exemplo. Outro problema é que grande parte dos servidores GNU/Linux não possuem ambiente gráfico, o que impossibilita usar este tipo de programa.


Abaixo, screenshots do GADMIN um dos mais famosos, a tela refere-se ao modulo para gerenciamento do DHCP Server:
Tela para configurar o Scopo, configuração principal do Servidor:


![Captura_de_tela.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780969389/kkTrnrjBj.png)



Tela para cadastro de maquinas, configurar ip fixo em computadores especificos:


![Captura_de_tela-1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780976023/VU_VhGEBt.png)





### "WEB"


Serviço instalado no Servidor, porém acessível via rede (ou disponível também acesso pela internet, caso firewall esteja devidamente configurado), ao instalar, é liberada uma determinada porta no servidor e o administrador irá acessar via navegador através de qualquer host dentro da rede. É o modo mais utilizado e mais comum de se encontrar em servidores instalados.


Vantagem: Mais rápido, acessível através de qualquer maquina, pode-se determinar qual porta será usada e habilitar o https (para os paranoicos), o servidor continua em modo texto, sem ambiente gráfico, não precisa realizar nenhuma configuração a mais no servidor.


Desvantagem: Nenhuma. Apenas precaução, pois você irá acessar através de qualquer maquina, cuidado ao fazer login, keyloggers, etc... Outro ponto é: caso a rede parar, você não terá acesso (mas caso sua rede pare, o dhcp será o último dos seus problemas....)


Existe diversas ferramentas para administração/gerenciamento e configuração de servidores GNU/Linux via Web. Algumas distribuições com foco para uso em servidores possuem suas próprias interfaces já instaladas, outras interfaces possuem pacotes para instalação nos mais diversos GNU/Linux e outros (como BSD por exemplo).


Exemplos:

A mais famosa das interfaces, com certeza é o Webmin, com diversos temas, muitos módulos (cada um para tipos de serviços e aplicações).

Abaixo, 4 das telas mais importantes do Webmin, no modulo de gerenciamento do DHCP Server:


![edição das configurações das sub-redes.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649780996200/_9dcpt-6R.png)



![tela inicial.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649781000227/gbp0qxgVO.png)



![zone dns.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649781005063/4_kGHBhSh.png)


Algumas distribuições focadas para servidor possuem suas próprias interfaces web,é o caso do ClearOS, OpenSUSE, Zentyal, PfSense, ZeroShell, Endian

Configuração principal do DHCP Server na interface web do GNU/Linux ClearOS (baseado em Red Hat / CentOS):



![clearOS-dhcp.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649781021291/xP2Ar4O4x.png)



Configuração principal do DHCP Server na interface web do GNU/Linux Endian Firewall:


![endian - dhcp.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649781028053/5Kn0l6-JI.png)



Configuração principal do DHCP Server na interface web do GNU/Linux ZeroShell:


![zero-shell.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649781035544/mEa5sRj8I.jpg)


Configuração principal do DHCP Server na interface web do PfSense (*BSD):

![pfsense_dhcp.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1649781043273/zMXs0TRcA.jpg)



Configuração principal do DHCP Server na interface web do GNU/Linux Zentyal:



![zentyal dhcp2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649781058655/4u9wnQYSr.png)


![zentyal dhcp3.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649781062991/iqWRn4j_k.png)




# Cliente DHCP no GNU/Linux

O ISC-DHCP também possui um cliente, como dito anteriormente no arquivo /var/lib/dhcp/dhclient.leases você tem um monitoramento, onde verifica as ações do cliente quando recebe o endereçamento.

Com o ISC cliente instalado, você terá o programa "dhclient", com ele você pode "resetar" uma placa com ip manual para pegar IP pelo dhcp server, removendo assim as configurações de IP feitas manualmente. Abaixo o resultado do comando: dhclient eth0 -v


```
root@linux:/home# dhclient eth0 -v
Internet Systems Consortium DHCP Client
For info, please visit https://www.isc.org/software/dhcp/
Listening on LPF/eth0/10:78:d2:1e:32:30
Sending on LPF/eth0/10:78:d2:1e:32:30
Sending on Socket/fallback
DHCPDISCOVER on eth0 to 255.255.255.255 port 67 interval 8
DHCPOFFER from 192.168.0.1
DHCPREQUEST on eth0 to 255.255.255.255 port 67
DHCPACK from 192.168.0.1
bound to 192.168.0.40 -- renewal in 270 seconds.
```



No log acima, meu servidor DHCP é o 192.168.0.1,meu host recebeu o IP 192.168.0.40 na interface eth0.



# Modelos de dhcpd.conf e Configurações Opcionais/Avançadas

## Exemplo 1:



```

ddns-update-style interim;
update-static-leases on;
ddns-domainname "teste.com.br.";
ddns-rev-domainname "0.168.192.in-addr.arpa.";

key DHCP_DNS {
algorithm hmac-md5;
secret "qLBp520GmSiCXEhM6Y6nyQ==";
};

zone 0.168.192.in-addr.arpa. {
primary 127.0.0.1;
key "DHCP_DNS";
}

zone teste.com.br. {
primary 127.0.0.1;
key DHCP_DNS;
}

authoritative;
default-lease-time 600;
max-lease-time 7200;
log-facility local7;
subnet 192.168.0.0 netmask 255.255.0.0 {
range 192.168.0.100 192.168.0.120;
option domain-name-servers 192.168.0.1;
option domain-name "teste.com.br";
option routers 192.168.0.1;
option broadcast-address 192.168.0.255;
option netbios-name-servers 192.168.0.1;
}

``` 




No exemplo 1 acima, o meu servidor dhcp também é o meu DNS server e proxy (firewall + roteador) e login (uma solução para pequena empresa, onde tenho um debian disponibilizando diversos serviços), nestas "configurações globais" as únicas mudanças são em relação ao DNS.




## Exemplo 2:

```
subnet 172.16.200.0 netmask 255.255.255.0 {
range 172.16.200.50 172.16.200.200;
range 172.16.200.230 172.16.200.253;
option domain-name-servers 172.16.200.1, 172.16.200.2;
option domain-name "teste.com.br";
option routers 172.16.200.254;
}
```


No exemplo acima, podemos colocar mais de um intervalo (range) de ips que serão oferecidos para os clientes, nele está configurado do final x.50 até o x.200 e depois do x.230 ao x.253.

Veja que isso já é uma facilidade no GNU/Linux, como exemplo, se fosse no Cisco eu deixaria o range do x.50 até o x.253 e colocaria o intervalo de x.201 ao x.229 como reservado.

## Exemplo 3:

```
subnet 192.168.0.0 netmask 255.255.255.0 {
range 192.168.0.50 192.168.0.200;
option domain-name-servers 192.168.0.1, 192.168.0.2;
option domain-name "teste.com.br";
option routers 192.168.0.254;
}

          host maq01 { 
          hardware ethernet 00:1e:8c:66:97:f1;
          fixed-address 192.168.0.3; option routers 192.168.0.1; }

          host maq02 { 
          hardware ethernet 00:50:56:c0:00:01;
          fixed-address 192.168.0.55; 
          option domain-name-servers 192.168.0.5; 
          option routers 192.168.0.9; }

          host maq03 { 
          hardware ethernet 00:55:56:c0:a1:51;
          fixed-address 192.168.0.102;}
```



Acima está sendo definido informações individuais para maquinas com ip fixo, ou seja, o que está nas configurações globais será aplicado em todas as maquinas, mas você pode deixar algumas com configurações diferentes e exclusivas para aquele host (já que o ip estará atrelado com o MAC address). 

No nosso exemplo, a "maq01" receberá um router (gateway / acesso a rede) diferente de todas as outras maquinas da rede, enquanto as outras maquinas recebem o router 192.168.0.254, a maq01 receberá o router 192.168.0.1.

A "maq02" além de receber um router diferente, também recebe outro servidor DNS para usar como sua configuração de rede.
A maq03 recebe os valores padrões definidos na configuração global.





## Exemplo 4:

```
subnet 192.168.0.0 netmask 255.255.255.0 {
range 192.168.0.50 192.168.0.200;
option domain-name-servers 172.16.200.1, 172.16.200.2;
option domain-name "teste.com.br";
option routers 172.16.200.254; 
}

          group servidores {
          option domain-name-servers 208.67.222.222;

                    host server01{
                    hardware ethernet 00:50:51:e1:00:01;
                    fixed-address 192.168.0.43;
                    option routers 192.168.0.9;}

                    host server02 {
                    hardware ethernet 00:80:54:c5:30:02;
                   fixed-address 192.168.0.39;
                    option routers 192.168.0.11;}
          }


          group roteadores {
          option routers 192.168.0.1;
          option domain-name-servers 192.168.0.41;

                    host router01 {
                    hardware ethernet 00:20:51:e1:00:21;
                    fixed-address 192.168.0.13;}
                    host router02 {
                    hardware ethernet 00:80:14:c5:32:22;
                    fixed-address 192.168.0.19; }
          }
           group maquinas {

                    host maq01 {
                    hardware ethernet 00:20:51:e1:00:50;
                    fixed-address 192.168.0.13; }

                    host maq02 {
                    hardware ethernet 00:80:14:c5:32:51;
                    fixed-address 192.168.0.19;}
          }
```

Uma outra opção é trabalhar com grupos, no exemplo acima, tenho as configurações globais, um grupo chamado "servidores" que tem uma alteração no DNS, ou seja a configuração sobre DNS que as maquinas do grupo "servidores" irão usar não são as definidas nas configurações globais, mas sim no inicio da configuração do grupo. 

Outro grupo, chamado "roteadores" também terá configurações exclusivas, neste caso, além do DNS diferente, o router também é outro. No grupo "maquinas" configurei 2 ip's fixos para maquinas da rede, tanto estas 2 quanto as demais maquinas da rede, receberão a configuração padrão definida no campo de configurações globais.





## Exemplo 5:

```
subnet 192.168.0.0 netmask 255.255.255.0 {
range 192.168.0.50 192.168.0.200;
option domain-name-servers 192.168.0.1, 192.168.0.2;
option domain-name "teste.com.br";
option routers 192.168.0 .254; 
}

subnet 172.22.0.0 netmask 255.255.255.0 {
range 172.22.0.30 172.22.0.100;
option domain-name-servers 172.22.0.5;
option domain-name "reload.br";
option routers 172.22.0.252;
}
```

No exemplo acima, tenho um trecho da configuração global, perceba que aqui tenho 2 sub-redes definidas com a qual o servidor irá realizar o endereçamento. Note que com isto, você deve indicar na configuração global as interfaces que o servidor irá trabalhar. 


## Exemplo 6:

A seguir tem-se um exemplo de como poderia ficar o arquivo de configuração. Nele foram configuradas 2 sub-redes, 4 ranges, grupos, hosts, informações globais e individuais. apesar de grande, não é complicado:


```
subnet 172.16.200.0 netmask 255.255.255.0 {
range 172.16.200.50 172.16.200.200;
range 172.16.200.230 172.16.200.253;
option domain-name-servers 172.16.200.1, 172.16.200.2;
option domain-name "teste.com.br";
option routers 172.16.200.254;
}

subnet 192.168.0.0 netmask 255.255.255.0 {
range 192.168.0.30 192.168.0.100;
range 192.168.0.120 192.168.0.200;
option domain-name-servers 192.168.0.5;
option domain-name "reload.br";
option routers 192.168.0.252;
}

 group servidores {
 option domain-name-servers 172.16.200.7;
 host server01 {
 hardware ethernet 00:50:51:e1:00:01;
 fixed-address 172.16.200.43;
 option routers 172.16.200.2; }
 host server02 {
 hardware ethernet 00:80:54:c5:30:02;
 fixed-address 172.16.200.39;
 option routers 172.16.200.9;}
 }

 group roteadores {
 option routers 172.16.200.1;
 option domain-name-servers 172.16.200.41; 
 host router01 {
 hardware ethernet 00:20:51:e1:00:21;
 fixed-address 172.16.200.13;}
 host router02 {
 hardware ethernet 00:80:14:c5:32:22;
 fixed-address 172.16.200.19;}
 } 

 group dmz {
 option routers 192.168.0.1;
 option domain-name-servers 192.168.0.32;
 host mail1 {
 hardware ethernet 00:20:51:e1:03:29;
 fixed-address 192.168.0.32; }
 host mail2 {
 hardware ethernet 00:80:14:c5:15:63;
 fixed-address 192.168.0.7;}
 }


 group hosts {
 host financeiro01 {
 hardware ethernet 00:20:51:e1:00:50;
 fixed-address 192.168.0.13;}
 host financeiro02 {
 hardware ethernet 00:80:14:c5:32:51;
 fixed-address 192.168.0.19;}
 }
```

# Conclusão

Ao utilizar Roteadores para prover o Serviço de DHCP em nosso ambiente, haverá duas situações, na primeira, os custos serão baixos e a configuração é simples e intuitiva, porém não suportará grandes mudanças e a evolução da pequena rede.

Além da questão da segurança, pois estes equipamentos não dispõem de complexos sistemas de segurança, e para destruir todas as configurações feitas, basta apertar o botão reset, localizado no corpo do equipamento.

Na segunda situação, os custos são altos, além da configuração e implantação serem complexas, deparando-se com as inúmeras possibilidades do IOS Cisco e seu preço, apenas utilizá-lo para DHCP não é viável, porém o equipamento já estaria pronto para sofrer modificações em sua configuração e suportar todo o crescimento da rede e sua evolução.

Uma vez em que a rede esteja configurada com o uso de um servidor de DHCP, esse passa a ser extremamente essencial para o funcionamento da rede, pois ele irá endereçar os equipamentos e fornecer informações como máscara, gateway e dns. Caso por algum motivo o servido fique inacessível, como por exemplo desligado, travado, desconectado, defeito no cabeamento, para na sua placa de rede, etc. toda a rede irá paralizar.

Nos casos em que o servidor dhcp não esta dentro da rede, a atenção é redobrada pois deve-se ficar atento também a possiveis problemas nos roteadores, por exemplo.

O DHCP consome poucos recursos do sistema, por isso ele não á aplicado como um servidor dedicado de dhcp, ou seja, um sistema/servidor completo em que somente prove este serviço. O mais comum desde pequenas a grandes redes de computadores, é um ambiente em que o sistema que prove o dhcp também disponibilidade e exerce outros serviços como firewall, proxy, sistema de gerenciamento e monitoramento, etc. 

O dhcp3-server usado no Linux é bastante rápido (desde que a configuração não seja muito complexa)  por isso costuma responder antes dos servidores DHCP usados nos servidores Windows e na maioria dos modems ADSL e/ou roteadores.









Referências Bibliográficas

Man Pages ;-)

DROMS, R. RFC 2131 Dynamic Host Configuration Protocol. Mai/1997. (Sobrepõe a RFC 1541 e RFC 1531) disponível em http://www.ietf.org/rfc/rfc2131.txt?number=2131

SCHRODE, Carla. Linux Networking Cookbook. O’reilly Media Inc., 2008. Rio de Janeiro: Alta Books.

VIGLIASI, Douglas. Redes Locais com Linux. 2. Ed. Florianópolis: Visual Book, 2007.

MORIMOTO, Carlos Eduardo. Redes - Guia Prático. Porto Alegre: Sul Editores, 2009.

MORIMOTO, Carlos Eduardo. Servidores Linux - Guia Prático. Porto Alegre: Sul Editores, 2009.

FERREIRA, Rubem E. Linux - Guia do Administrador de Sistema. São Paulo: Editora Novatec Ltda., 2003.

STANEK, William R.. Microsoft Windows Server 2003 - Guia de Bolso do Administrador. Porto Alegre: Bookman, 2006.

CARMONA, Tadeu. Treinamento Profissional em Redes – Guia Avançado de Manutenção e Auditoria em Rede de Computadores. São Paulo: Digerati Books, 2006.

ODOM, Wendell. CCNA ICND1 Exame 640-822. 2 Ed. Editora Alta Books, 2008

FILIPPETTI, Marco Aurelio. CCNA4.1 Guia Completo de Estudo. 1 Ed. Editora Visual Books, 2008

Distrowatch http://www.distrowatch.com

ISC – Internet Systems Consortium, http://www.isc.org

Hugo Azevedo http://www.hugoazevedo.eti.br



