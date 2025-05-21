---
title: "Administração de sistemas e Deploys"
datePublished: Tue Oct 11 2016 20:54:09 GMT+0000 (Coordinated Universal Time)
cuid: cl0smdorn02a549nv2w3y21lo
slug: ansible-chef-fabric-puppet-ou-salt
canonical: https://www.esli-nux.com/2016/10/administracao-de-sistema-e-deploys.html
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1647376680438/oGQNwjyj8.png
tags: continuous-integration, continuous-deployment, automation, devops, deploy

---

Já pensou num sistema de gerenciamento com fácil configuração e administração na qual ele pode ser usado para automatizar e organizar todas as suas tarefas de configuração, administração, gerenciamento, etc... dos sistemas operacionais em sua rede de computadores? Como updates, alteração de configs, backups, reboots, criar/clonar/parar instâncias... todas aquelas tralhas que são mantidas nos crontabs em cada um dos servidores... Bem como os diversos S.O. e até mesmo os deploys de aplicações e projetos.

Na falta de um, temos vários caras que fazem isto. E neste post vou abordar 5 deles: Ansible vs Chef vs Fabric vs Puppet vs Salt.

O Ansible foi lançado no inicio de 2012, juntamente com o Salt.
Os mais velhos são o Chef e o Fabric, ambos de 2009, Puppet vem desde 2005.

Há também outros nomes que não abordarei, como o Otter (Comercial, com suporte para Windows e seu PowerShell), o CFEngine, o (R)?ex ou simplesmente Rex (de "Remote Execution"), Capistrano.

Há uns 7 anos atrás eu administrava a rede interna (escritório) bem como o ambiente de desenvolvimento e produção (onde estava os aplicativos sendo usado pelos clientes) de uma desenvolvedora de software.

Basicamente tínhamos uns databases MySQL (onde os aplicativos dos clientes se conectavam), servidores web em nuvem com outra aplicação... Na rede interna havia uma replica deste ambiente (virtualizada com Xen server), um servidor SIP/Voip Asterisk, um Firewall e um Samba (como file server, perfil remoto, AD domain controller) e por fim um cara para armazenar backups. Tudo estava em Debian 4 (o 5 havia ficado estável há pouco tempo e eu estava atualizando todos). Havia uma bagunça de scripts na cron de cada server, mandando backups para um servidor (muito mal feito, mal escrito....) enfim...

Comecei criando um script em bash/awk (usando ssh e expect) que ficava na maquina de backup, se conectava nas demais e transferia para sí os arquivos necessários... Depois aumentei o script colocando verificação dos arquivos (hash, verificação do tamanho antes do backup e depois)... Inseri um sendmail para enviar um email para mim informando os arquivos de backup (sucesso, tamanho, inicio e fim da execução, ...).

Quando migrei tudo para o Debian 5, inseri um parâmetro para este script onde mandava um apt-get update e upgrade para os hosts.
Neste momento de felicidade, eu tinha na mesma crontab todos os backups (incremental, diferencial, full de databases, web, firewall, sip, config files...), além das atualizações dos S.O.s... Tudo feito pelo mesmo script gigante e seus vários parâmetros.

Não lembro qual a necessidade que tive, mas passei a procurar algo na internet que fosse semelhante, mas tivesse algo mais 'pronto' ou 'fácil', foi quando descobri o Fabric! Passei a estudar o Python (e sim, gostei muito), antes que pudesse migrar meu lindo frankenstein, acabei solicitando minha demissão e indo trabalhar em outra empresa (mas calma!! deixei tudo bem documentado lá... creio que quem prosseguiu não teve dificuldades).

Outro bom exemplo foi quando tive que varrer diversos hosts (quase um /24 inteiro) buscando determinados arquivos que tivessem uma extensão especifica e permissão de execução (pensei em usar o Fabric na época, mas uma gambiarra em bash me pareceu mais rápida de fazer, já que era uma emergência e sem chances de usá-la novamente).





## Infraestruture As a Code

Trabalhar em produção hoje resulta em implantações continuas em ambientes distribuídos, infraestrutura descentralizada, serviços em nuvem, virtualização...

Ter uma maneira de automatizar a configuração e manutenção de tudo é uma grande maravilha. Criar novas instâncias (ou clonar), restaurar backups de projetos e já subir o ambiente apenas executando um 'job'... sem dores de cabeça ou trabalhos repetitivos na qual um SysAdmin já está de saco cheio de fazer (tar xzvf, scp, systemclt, update, ...). Pegar os arquivos da equipe de dev e subi-los para produção (ou homologação) bem como a automação disto (e sua validação)...

Ferramentas de gerenciamento de implantação, ferramentas de automação de deploy e ferramentas de gerenciamento de configuração (dentre outros nomes dados) são projetados para esta finalidade.

Eles permitem que você use 'receitas', 'cookbooks', 'playbooks', modelos, arquivos, topfile, fabfile ou qualquer que seja o nome dado a estes scripts para simplificar a automação e orquestração em seu ambiente para fornecer um padrão, a implantação consistente.

Há várias considerações para manter em mente ao escolher uma ferramenta neste espaço. Um deles é o modelo para a ferramenta. Alguns exigem um modelo server-client, que usa um ponto de controle centralizado para se comunicar com máquinas distribuídas, enquanto outros podem (ou não) operar em um nível local. Outra consideração é a composição do seu ambiente. Algumas ferramentas são escritas em diferentes línguas e suporte para determinados S.O. ou configurações. 

Poupar dor de cabeça: Certifique-se de que a escolha da ferramenta a ser usada está alinhada com o seu ambiente e as habilidades específicas de sua equipe. Bem como sua capacidade de aprendizado, tempo disponível e expectativa de uso. 









## Ansible

Ansible é uma ferramenta de código aberto usado para implantar aplicativos para nós remotos e provisionar servidores de uma forma repetível. Ele dá um framework comum para enviar as aplicações usando uma configuração de push (disparo), embora você pode configurá-lo como mestre-cliente, caso queira. Ansible é construído sobre 'playbooks' que podem ser aplicados a uma ampla variedade de sistemas.

Não requer instalação de agentes nos hosts, baseia-se em conexões SSH
Estes playbooks são criados usando YAML, tornando-o fácil de aprender e com o formato destes playbooks simples e estruturados.

Base de código simples e usa um recurso de registro de variável que permite tarefas para registrar variáveis que serão usadas em tarefas posteriores
Porém, como 'contra' ele pode tornar tarefas simples em desgastantes pois necessita que seja registrado as variáveis (mesmo para funções básicas). Pode ficar difícil em ver valores das variáveis dentro dos playbooks.





 


## Chef

Chef é uma ferramenta de código aberto para gerenciamento de configuração, focada no desenvolvedor para sua base de usuários. Ele opera como um modelo mestre-cliente, com uma estação de trabalho separada necessária para controlar o mestre. É baseado em Ruby, na qual será usado para escrever a maioria dos elementos. O projeto Chef é transparente e baseada em seguir as instruções que é dado, o que significa que você terá que certificar-se de suas instruções são claras.

Chef é bom para as equipes e ambientes com foco no desenvolvimento. É bom para as empresas que procuram uma solução mais madura para um ambiente heterogêneo.
Possui uma rica coleção de módulos e receitas de configuração. Centralização com git e usa a ferramenta 'Knife' para inserir os agentes nos hosts via ssh, melhorando um pouco a sua abordagem, porém tomando mais tempo para instalação, não é uma ferramenta simples e pode trazer grandes bases de código e complicações ao ambiente.

Pesa contra ele a necessidade de aprendizagem de Ruby e codificação processual.
Chef oferece suporte completo para ferramentas de desenvolvimento orientado a testes e uma abordagem de provisionamento que permite uma grande flexibilidade na forma como os recursos são implantados.

Chef é mais barato, mais fácil de implantar e escalar comparado ao Puppet. Porém, neste quesito, perde feio para Ansible e Fabric.




 

## Fabric

Baseado em Python, sem necessidade de client e assim como o Ansible, trabalha através de SSH.

Seu uso principal é para a execução de tarefas em vários sistemas remotos, mas também pode ser estendido com plugins para fornecer funcionalidade mais avançada. 

Simples, fácil e rápido... É o melhor para implantar levando em consideração o tempo gasto (com certeza, é o mais rápido para isto). Pode ser usado por quem está iniciando nesta área ou quem possui um ambiente pequeno, sem muitas chances de crescimento exponencial em pouco tempo e também para quem crê que usar outra ferramenta mais vai trazer contratempos do que agilizar tarefas (como estudar cada ação para criá-la nas ferramentas de automação, por exemplo).

Bom em implementar aplicações escritas em qualquer idioma. Ela não depende da arquitetura do sistema, mas sim do sistema operacional e gerenciador de pacotes.
Apesar da curva de aprendizado ser bem menor do que os demais, pesa contra ele a necessidade de conhecimentos em python, para no mínimo, entender ou dar manutenção nos scripts.






## Puppet

Uma das ferramentas mais antigas, sendo portanto uma das mais adotadas e com maior número de scripts disponíveis. 

Empresas grandes, com restrições de infraestrutura e conceito conservador optam mais pelo uso do puppet, buscando uma maturidade e estabilidade, bem como garantias, pois veem estas ferramentas com olhos tortos (alérgicas a vanguardismo).
Ele é executado como uma configuração master-client e utiliza uma abordagem orientada a modelos. O Puppet funciona como uma lista de dependências, o que pode tornar as coisas mais fáceis ou mais confusas, dependendo de sua configuração.
Ele é baseado em Ruby e utiliza uma linguagem de script denominada 'DSL' e se aproxima do JSON.

Ele tem a interface mais madura e evoluída e é executado em quase todos os S.O., possui instalação simples bem como sua configuração inicial.
Porém para tarefas mais avançadas, você precisará usar o CLI, que é baseada em Ruby (ou seja, você vai ter que entender o Ruby). Seu design não se preocupa muito em simplificar, portanto sua curva de aprendizagem pode ser grande e difícil, além de crescer juntamente com as suas necessidades.






## Salt

O Saltstack  é uma ferramenta baseada em CLI que pode ser configurado como um modelo master-client ou um modelo não-centralizado.

Com base em Python, ele oferece um método de envio por distribuição e um método de comunicação via SSH com os clientes. Permite também o agrupamento de clientes e modelos de configuração para simplificar o controle do ambiente.  O Salt é uma boa escolha se escalabilidade e resistência são uma grande preocupação. É bom para administradores de sistema, graças à sua usabilidade.


Usa YAML para arquivos de entrada, saída e configuração, é fácil ver o que está acontecendo 'por dentro' (ao contrario do Puppet, por exemplo). Com organização e uso simples, associado com alta escalabilidade e resiliência no modelo principal e com níveis hierárquicos.

Pesa contra ele a dificuldade de configurar e a curva de aprendizagem para novos usuários, não possui uma Web UI nos mesmos 'níveis' (features, etc...) dos outros. Limita-se ao Linux como S.O. client.





## YAML x Ruby x Python x JSON

Um dos pontos a considerar é qual a aprendizagem para Ruby, Python, Json e YAML (e a possibilidade de escrever scripts auxiliares nas duas primeiras linguagens ou uso de outra complementar).

YAML é um formato de serialização, codificação de dados legíveis, criado em 2001 e inspirado em linguagens como XML, Python, Perl,..

YAML é um acrônimo recursivo que significa "YAML Ain't Markup Language" (em português, "YAML não é linguagem de marcação"). No início do seu desenvolvimento YAML significava "Yet Another Markup Language" ("Mais outra linguagem de marcação"). 
O YAML foi criado na crença que todos os dados podem ser representados adequadamente como combinação de listas, hashes (mapas) e dados escalares (valores simples). A sintaxe é relativamente simples e foi projetada tendo em conta que fosse muito legível, mas que também fosse facilmente mapeada pelos tipo de dados mais comuns na maioria das linguagens de alto nível. 

Além disso, YAML utiliza uma notação baseada em identação e um conjunto de caracteres sigil distintos dos que são usados pelo XML, fazendo com que as duas linguagens sejam facilmente compostas uma na outra.

Para um SysAdmin é mais racional ir para uma aprendizagem de python (pois provavelmente já teve experiências com esta linguagem, além de Perl, Bash, awk e outras interpretadas) e também o provável uso desta linguagem para suas aplicações ou ferramentas do dia-a-dia do que comparado ao Ruby (apesar de que é inspirado no python/perl, é mais novo e não possui a mesma gama de aplicações do que as linguagens que mencionei anteriormente), um exemplo disto é a possibilidade de inserir scripts em Python nos serviços da Amazon (Lambda).



![CMTs1.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1647377776927/H_jAe2HEa.jpg)



|   | Ansible | Chef | Fabric | Puppet | Salt   |
|-----------------|--------|-------|-------|---------|------|
| Liguagem base | python | ruby+DSL | python | Ruby+DSL | python |
| Interface Web   | AWX, Rundeck, Ansijet (ansibot), Semaphore, Foreman  | Opscode-manage, foreman | Fabric Bolt, Aurora |  Foreman, PuppetBoard, PuppetExplorer | SaltPad, Foreman |
| Versão Paga   | Ansible Tower | Chef Automate  |     --      | Puppet Enterprise  | SaltStack Enterprise |
| Agent/Client   |    não        |    sim        |    não       |     sim          |    não      |






## Conclusões


O gerenciamento de configuração ou a ferramenta de automação que você usar dependerá de suas necessidades e preferências para o seu ambiente.
Chef e Puppet são algumas das opções mais antigas e mais estáveis, tornando-os bons para as grandes empresas e ambientes que valorizam a maturidade e estabilidade sobre a simplicidade.

Ansible, Fabric e Salt são boas opções para aqueles que procuram soluções rápidas e simples ao trabalhar em ambientes que não precisam de suporte para funcionalidades peculiares, sendo que Fabric e Salt possuem mais scripts e facilidades para sistemas GNU/Linux, e o Fabric é excelente para ambientes menores e aqueles que procuram uma solução sem complexidade para aprendizagem com rápida implantação.

Chef e Puppet são ferramentas bastante maduras, amplamente utilizadas e com boa documentação e grande comunidade de usuários ativos. Eles possuem grandes empresas como clientes - Chef é usado pela GE, Disney, e Facebook, enquanto Puppet conta com Salesforce, Sony e Google entre seus clientes.

Existe uma máxima onde afirma que:

"Chef foi feito para desenvolvedores, enquanto Puppet para os administradores de sistema"

Porém de acordo com Rich Morrow, "Chef tenta dar mais poder ao usuário, Puppet coloca trilhos de segurança em torno deles", ou seja, o Chef quer ajudá-lo a fazer as coisas, o Puppet quer ajudá-lo a não cometer erros.

Chef é mais amado pelos desenvolvedores porque: Permite rodar Ruby, enquanto Puppet crê que isto vá atravancar as coisas, somando-se a complexidade do desenvolvimento.

Puppet limita a sua oferta de código aberto, enquanto Chef fornece capacidades de código aberto completa, incluindo relatórios grátis e análises.

Puppet e Chef possuem grandes bibliotecas de 'receitas' (scripts) prontos devido aos seus tempos de existência, porém a adoção crescente do Ansible e Salt fazem com que eles possuam bibliotecas novas e com soluções para tarefas atuais na qual para os demais pode ser que ocorra divergência de versões ou atualizações. Em diversos sites e blogs você encontra a comparação de Ansible vs Chef, Puppet vs Ansible ou comparações entre estes 3 e/ou adicionando o Salt (sim, Fabric é bastante deixando de lado).

Em quase todas estas comparações, a escolha pende para o Ansible... A quantidade de "pros" é bem maior e mais valiosa do que as listas de "contras" (na lista de contras, caem argumentos como: ser muito novo, não ter suporte ao Mac OSX).


Já usei o Fabric em determinadas situações e já trabalhei num ambiente com Chef (onde a equipe de infra tinha um longínquo roadmap tentando instalá-lo por completo, batendo cabeça e muitas vezes tendo que executar algumas correções a mão ou acessar o host para confirmações...). Atualmente coordeno o T.I. numa das maiores empresas do seu ramo e, sem tempo para ficar brincando com Ruby, decidimos (quase impondo kkk) pelo Ansible. 
Resultado: 1 dia para implantar e criar algumas configurações personalizadas e ponto!! 4 redes diferentes, 2 tipos distintos de virtualização, todo o ambiente Amazon, VPSs num outro hosting, firewalls e diferentes aplicações, SGDBs e S.O.

 
[Aqui um vídeo(inglês) com o tema "**Chef vs. Puppet vs. Ansible vs. Salt - What's Best for Deploying and Managing OpenStack?**"](https://www.openstack.org/summit/tokyo-2015/videos/presentation/chef-vs-puppet-vs-ansible-vs-salt-whats-best-for-deploying-and-managing-openstack)




### Links com comparações e discussões sobre as ferramentas

https://dantehranian.wordpress.com/2015/01/20/ansible-vs-puppet-hands-on-with-ansible/

http://probably.co.uk/puppet-vs-chef-vs-ansible.html

https://www.upguard.com/articles/ansible-vs-chef

https://www.upguard.com/articles/ansible-puppet

https://www.quora.com/Configuration-Management-Which-should-I-choose-Chef-Puppet-Ansible-SaltStack-Docker-or-something-else

http://www.infoworld.com/article/2609482/data-center/data-center-review-puppet-vs-chef-vs-ansible-vs-salt.html



Outros Links e referências:

Otter 
http://inedo.com/otter

Rex 
http://rexify.org/

https://pt.wikipedia.org/wiki/YAML
