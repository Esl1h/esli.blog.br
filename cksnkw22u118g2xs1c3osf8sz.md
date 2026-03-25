---
title: "Etckeeper"
datePublished: 2019-01-14T18:02:00.000Z
cuid: cksnkw22u118g2xs1c3osf8sz
slug: etckeeper
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1629658675715/BupulNdNA.png
tags: linux, github, git

---

%[https://youtu.be/riyXmtKO0L4]


Uma das muitas vantagens dos sistemas operacionais Linux, UNIX e similares é que tudo é um arquivo e que a maior parte da sua configuração é feita através de arquivos de texto, permitindo que você leia e escreva facilmente para eles com qualquer ferramenta que você escolher. 

Há diversas ferramentas para monitorar, automatizar e controlar as configurações dos seus sistemas e com elas você poderá:

Verificar as mudanças recentes no ambiente, restaurar caso algo seja deletado, reverter com maior segurança caso uma alteração gere erros, compartilhar facilmente com um time as mudanças ocorridas, nível de segurança garantindo a integridade das configurações quando seu ambiente está grande o suficiente para não saber nada mais 'de cor' e acabar com milhões de copias de arquivos de configurações (quem nunca logou num server, e dentro de algum diretório do /etc encontrou milhares de versões com sufixos ou prefixos bkp, v1, v2, old, etc...).



Então, já pensou em usar o git (ou outro) para revisar ou reverter alterações feitas no /etc ou até mesmo fazer o "push" do repositório para outros lugares?



O etckeeper é uma coleção de ferramentas que permite o /etc ser armazenado em um repositório git, mercurial, bazaar ou darcs. Por padrão o etckeeper usa o git.



Além de ser simples de usar, o etckeeper é modular e altamente configurável e se você já conhece o git ou svn será mais fácil ainda 😉



Entre os recursos adicionais que ele possui, o etckeeper conecta-se a gerenciadores de pacotes como o apt para confirmar automaticamente as alterações feitas  durante as atualizações de pacotes. Rastreia metadados de arquivos que o git normalmente não suporta, mas que é de extrema importancia para o /etc como por exemplo as permissões do /etc/shadow.


Outras ferramentas, como o gerenciamento de configuração e o gerenciamento de pacotes da distribuição, também controlam as configurações, mas não rastreiam todos os arquivos no controle de versão. 



Os sistemas de gerenciamento de configuração (CMS), como Puppet, Chef e Ansible, não rastreiam todos os arquivos no /etc. Para muitos arquivos, eles apenas verificam se certos conteúdos estão no lugar e ignoram o restante do arquivo. 
Por exemplo, o Puppet irá verificar se um usuário existe em /etc/passwd, mas irá ignorar as mudanças nas contas que ele não está gerenciando. Quando é feita uma atualização de algum pacote com serviço já configurado e rodando, ele irá trazer um novo arquivo de configuração, informará que já existe outro, mas não rastreia as modificações, tendo que o sysadmin faça a comparação do novo e aproveite os novos recursos, ignore, substitua, enfim...



O Etckeeper irá rastrear tudo, exceto o diretório do repo e o que você mandar ignorar no .gitingnore, o etckeeper vai auxiliar o controle de versão rastreando metadados importantes no arquivo .etckeeper.



O Etckeeper aumenta o sistema de controle de versão subjacente rastreando esses metadados importantes em /etc/.etckeeper. Mais uma vez, esse arquivo é rastreado no sistema de controle de versão. O Etckeeper também possui ganchos para trabalhar com o sistema de gerenciamento de pacotes e fazer o check-in das mudanças após as instalações e atualizações dos pacotes.




A versão mais recente é a 1.18.16
Para instalar ele basta verificar o seu gerenciador de pacotes da sua distribuição (baseados em RedHat e Debian já possuem como o CentOS e Ubuntu). 
Caso prefira, o fonte está no git https://git.joeyh.name/index.cgi/etckeeper.git (nota: antes do 'make install', edite o conf para a sua distribuição).




Uma vez instalado, use o init para inicializar o repositório e, em seguida, verifique o estado atual. 

```
$ sudo etckeeper init 
$ sudo etckeeper commit -m "Checkin inicial" 
``` 

Uma vez inicializado, o /etc será um repositório em seu sistema de controle de versão, assim como qualquer outro repo iniciado.
Você pode digitar os comandos do VCS diretamente (como git status, git commit, etc...) ou usar o etckeeper:
``` 
$ sudo etckeeper vcs git status
$ sudo etckeeper vcs git commit
``` 

Se usar alguma ferramenta como o Gitkraken ou o SmartGit, irá conseguir abrir normalmente.

Abaixo, visualizando com o Gitkraken, habilitei o etckeeper no meu notebook e como 'remote' coloquei num repo privado no github.com.



Site: https://etckeeper.branchable.com/
