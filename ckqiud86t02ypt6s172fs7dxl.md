---
title: "Podcast Sysadmin #02: GitOps - Operações por Pull Requests"
datePublished: Mon Feb 15 2021 22:15:57 GMT+0000 (Coordinated Universal Time)
cuid: ckqiud86t02ypt6s172fs7dxl
slug: podcast-sysadmin-02-gitops-operacoes-por-pull-requests
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1625254232518/U0xrHjXI0.jpeg
tags: sysadmin, podcast, podcasts

---

%[https://anchor.fm/esl1h/episodes/Podcast-Sysadmin-02-GitOps---Operaes-por-Pull-Requests-eqf4m6/a-a4lb7vk]

Se um operador humano precisa estar em contato com seu sistema durante as operações normais, você tem um bug.A definição de normal muda a medida que a empresa cresce.  
Se estamos aplicando a engenharia em processos e soluções que não sejam automatizáveis, continuaremos precisando contratar pessoas para dar manutenção no sistema. Se precisarmos contratar pessoas para fazer o trabalho, estaremos alimentando as maquinas com sangue, suor e lágrimas de seres humanos.  

  
Problemas que não se resolvem com a IAAC (Infraestrutura como Código):

  
Segundo artigo do Dmitrii Evstiukhin da Provectus no site Dzone.com todas as abordagens para gerenciamento de configuração de infraestrutura possuem problemas.  
E alguns não podem ser resolvidos pela IaaC. como por exemplo:  

*   Infra-estrutura implícita, seu estado e configuração. Às vezes, você precisa realizar uma investigação em grande escala para descobrir onde e como o aplicativo está sendo executado.
*   Falta de histórico das mudanças. Pode ser ainda mais difícil descobrir como alguns aplicativos foram desenvolvidos em primeiro lugar e quem os gerencia agora.
*   Falhas de segurança nos ambientes. Criados para facilitar o acesso do administrador às ferramentas de Entrega Contínua, esses buracos garantem a velocidade e a conveniência do desenvolvimento, mas colocam em risco os aplicativos e seus usuários.
*   A dependência do sucesso das operações no estado anterior. Uma variável verdadeiramente imprevisível, que, no entanto, implica o sucesso de todo o projeto.
*   Dificuldades de recuperação de desastres. Com processos manuais / semi-manuais, a recuperação de desastres requer uma estratégia, que nem sempre é o caso.
*   Consistência eventual da configuração. Mesmo com o uso da configuração centralizada, a configuração real e a declarada podem derivar com operações manuais.

  
  
  

GitOps
------

  

"GitOps" é um termo originalmente criado pela Weaveworks em 2017 para definir o novo método de usar a ferramenta para conduzir operações.

GitOps em sua raiz é DevOps com IaaC, gerenciamento de configuração, integração continua e etc... mas com o git sendo o centro destas operações.  
Num ambiente padrão, tudo é desenvolvido em "manifestos", o push é feito enviando para o Git e a partir disto (com o pull request aprovado) há o deploy no ambiente.  
  
É frequentemente associado à configuração do Kubernetes, porque o Kubernetes é usado em todos os lugares agora, e o GitOps é melhor para gerenciar a configuração do k8s.  
  
Mas com o GitOps não há o kubectl nem scripts! (nem tanto rsrsss)  
  
Usando o Git como nossa fonte de verdade (na verdade, o git é única fonte de verdade), podemos operar quase tudo.  
Como por exemplo, controle de versão, histórico, revisão por pares e reversão acontecem no Git sem precisar mexer em ferramentas como o kubectl.  
  
Todas as operações são feitas por requests (além de pipelines de compilação e delivery), ou seja, somente é usado automação baseada em push/pull.  
As ferramentas Diff detectam qualquer divergência e nos notificam via alertas do Slack;  
Ferramentas de sincronização permitem convergência  
Logs de reversão e auditoria também são fornecidos via Git    
  
No caso do Kubernetes, usamos o controle de versão não apenas para o código, mas também para os arquivos YAML que definem as implantações, os serviços, os daemonets da Kubernetes, etc. Também usamos o Terraform e o Ansible para provisionar o Kubernetes no Amazon, e eles também são controlados por versão no Git.  
  
A abordagem não se limita a Kubernetes, ou Ansible e Terraform.

  
O GitOps determina princípios de alto nível e não ferramentas em sí, se você quiser, poderá usar como gerenciamento de configuração, um sistema baseado todo em shell scripts (bash), como é o [https://aviary.sh](https://draft.blogger.com/blog/post/edit/128164995609276812/2318386785508736164#)  

  
Os quatro princípios principais:  

1.  O sistema inteiro deve ser definido declarativamente.
2.  Versão de toda a definição de sistema no Git.
3.  Aplicar automaticamente alterações aprovadas à infraestrutura.
4.  Agentes de automação especiais garantem a convergência do sistema

  
  
  
Colocando na linguagem técnica: Antes de 2006, se você precisava de um LAMP (ambiente com Linux, Apache, MySQL e PHP) em funcionamento, era necessário fazer o SSH no servidor, instalar ou compilar pacotes, fazer todas as configurações no PHP, banco, servidor web, corrigir direitos de acesso e iniciar os serviços.  

Quando você precisava do segundo servidor, fazia o SSH novamente e precisava fazer o mesmo do zero, com novas versões de pacotes.

Se você precisasse de 50 servidores, a quantidade de trabalho aumentaria em 50x.  
  
Foi quando todos entenderam que algo precisava mudar e a abordagem Infraestrutura como Código se tornou popular.  

Com o GitOps, passamos dos servidores para o estado desejado, além de contar com a automação para atingir esse estado.  
Trabalhamos com a definição de infraestrutura no Git, em vez de scripts.

Depois de mesclada, a automação do GitOps cuidará de todas as configurações necessárias e garantirá que o aplicativo permaneça em funcionamento se algo der errado.  
  
Lembre-se de que não nos importamos com o que exatamente a automação fará. Estamos em um nível totalmente novo de abstração agora.  
  
O GitOps é um novo paradigma, um passo adicional na evolução da administração do sistema. Mudamos o foco das operações diárias da própria infraestrutura para sua representação no repositório Git.  
  
Kubernetes se encaixa perfeitamente nesse novo paradigma. Por ser declarativo e com apenas uma etapa adicional - a instalação do agente GitOps - você pode implementar a abordagem GitOps para o Kubernetes.  
  
O GitOps para Kubernetes é melhor começar a experimentar e se acostumar com a conveniência e a segurança do método. Em seguida, mudaremos todas as suas soluções de IaC para o GitOps.  
  
  
  

### **Adoção**

  
Obviamente, o GitOps não é a panacéia para todos os problemas de infraestrutura. Tem seus desafios.  
  
O primeiro desafio que você enfrenta ao decidir adotar a abordagem GitOps é o número de decisões a serem tomadas. Como o GitOps introduz a mudança de foco e determina princípios de alto nível, você decide como gerenciar manifestos, dividir repositórios e implementar provisionamento e atualizações de ambiente.  
  
Automação, em geral, é algo que você mesmo descobriu. Para o Kubernetes, você pode encontrar vários agentes do GitOps, como o ArgoCD ou o Weave Flux - duas soluções mais maduras.

  
O maior desafio da abordagem GitOps é a mudança cultural, no entanto.  
O GitOps me lembra o DevOps, pois nos leva a mudar nossos hábitos, mentalidade e rotinas diárias de trabalho.

  
Essa mudança de foco é difícil de executar a princípio, pois todos os desenvolvedores e administradores de sistema estão acostumados a trabalhar diretamente com servidores.  
  
E agora estamos dizendo a eles que tudo o que eles podem fazer é solicitar via Pull Request. Você não pode fazer o SSH no servidor e aplicar um hotfix ao aplicativo ou à sua configuração. Ou, deixe-me colocar desta maneira: você pode fazer isso, mas a automação o reverterá, mesmo assim, forçando você a seguir o processo.  
  
Essa mentalidade começou a mudar quando tudo começou a ficar em contêiner com o Kubernetes. Como o aplicativo no contêiner é imutável, você não pode simplesmente corrigi-lo. Você precisa criar relações públicas e criar uma nova imagem.  
  
Mas isso preocupava apenas desenvolvedores, os administradores de sistema ainda podiam interagir diretamente com o sistema e fazer alterações na configuração.  
  
Agora, com a abordagem GitOps, se você quiser alterar alguma coisa - faça um pull request, mesmo se você for o administrador do sistema.  
  
Essa mudança cultural é fundamental para a implementação do GitOps.  
  
Exige muito esforço da equipe e ainda mais esforço de líderes e gerentes.  
  
  
_**Recompensas pelo esforço**_  
Então, o que vamos conseguir por todo esse esforço?  
  
**Transparência.**

Com o GitOps, você obtém uma infraestrutura de auto-documentação e implantações auto-descritas. Você sempre sabe onde implantou algo. Você tem um histórico de alterações com seus autores gratuitamente.  
   O estado da infraestrutura não é mais um conhecimento sagrado. Além disso, você pode criar plataformas de autoatendimento muito mais fáceis.  
  
  
**plataformas de autoatendimento**

rollback fáceis e recuperação de desastres. Se você tiver uma representação completa de sua infraestrutura, poderá recriar todo o sistema do zero em questão de minutos. E, se sua atualização mais recente não correu bem, a única coisa que você precisa fazer é "reverter o git".  

  
**Segurança aprimorada**

O GitOps melhora a segurança de várias maneiras diferentes. Altere a aplicação de fluxo e aprovações no nível do repositório Git. Você pode dividir e dividir o acesso da maneira que desejar, por diferentes estruturas de repositório.  

**Segurança do ambiente de tempo de execução**

você não precisa passar o acesso de administrador do Kubernetes a nada fora do Kubernetes, porque o agente GitOps gerencia o cluster por dentro.  

  
  
**Precisão das operações.**

Sempre obtemos o que é descrito no Git. Não precisamos mais confiar no terceiro com o script.  

  
  
  
**Processo de integração mais rápido.**

Com toda a infraestrutura e configuração de aplicativos no Git, é muito mais fácil integrar novos membros da equipe. Com mensagens de confirmação adequadas, as novas contratações podem se familiarizar com as operações e processos diários muito mais rápido e fácil.  
  
  
Quando começar a implementar a abordagem GitOps?  
Depende. Talvez você tenha que começar a fazer isso ontem; por exemplo. se você já possui um cluster Kubernetes e não tem uma imagem clara do que e onde está implantado.  
  
  
Como fazer isso? Fácil - são apenas duas etapas:  

1.  Criar uma representação de infraestrutura no Git
2.  Comece a sincronizar automaticamente a infraestrutura com sua representação

  
  
  
  

Links:  
  

[https://www.gitops.tech/](https://draft.blogger.com/blog/post/edit/128164995609276812/2318386785508736164#)

  
[https://github.com/fluxcd/flux](https://draft.blogger.com/blog/post/edit/128164995609276812/2318386785508736164#)

  
[https://www.weave.works/blog/what-is-gitops-really](https://draft.blogger.com/blog/post/edit/128164995609276812/2318386785508736164#)

  
[https://www.weave.works/blog/gitops-operations-by-pull-request](https://draft.blogger.com/blog/post/edit/128164995609276812/2318386785508736164#)

  
[https://dzone.com/articles/the-why-and-when-of-gitops](https://draft.blogger.com/blog/post/edit/128164995609276812/2318386785508736164#)  
  
Musica de inicio: [https://www.bensound.com](https://draft.blogger.com/blog/post/edit/128164995609276812/2318386785508736164#)