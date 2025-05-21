---
title: "Programando no Chromebook: Melhores IDEs para o ChromeOS"
datePublished: Tue Nov 05 2019 15:36:00 GMT+0000 (Coordinated Universal Time)
cuid: ckqitoj6802w0u9s12ct9gnyn
slug: programando-no-chromebook-melhores-ides-para-o-chromeos
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1625017865134/5KidIZxr9Q.png
tags: code, linux, github, ides, chrome

---


Olá,

Quem já acompanha meu blog sabe que possuo um notebook de baixo custo com Intel N3010 e 4Gb de ram, comprado numa Black Friday 2016 e que a Positivo enviou com o Mandriva (na qual estou escrevendo este artigo =) Até uns 3 meses atrás estava usando Debian com Xfce e mudei para o Manjaro Xfce.

Bom, voltei ao ChromeOS!

Dentre minha necessidades atuais, a principal é um editor de código ou uma IDE que consiga suprir o que preciso.

No meu trabalho diário basicamente preciso de bash, python, ansible e terraform. Raramente devido a AWS acabo precisando mexer com json e js (policies, lambda, …).

Parti em busca de uma IDE que consiga principalmente me deixar confortável :-)

Fiz uma lista grande e comecei a testa-los.


Antes, confira um review rapido sobre a transformação de um notebook de baixo custo em um Chromebook:

%[https://youtu.be/0nCP6a9OISg]


**IDE por assinatura:**
ShiftEdit: US$6/mês é restritivo. 
Outras IDEs excelentes também foram removidas da comparação, como o **Cloud9**, **Codio**, …

O tempo voa! E editores cujo ultima atualização foi em 2014 retirei da lista! Além de outros que na própria descrição informa que o app não está 100% ainda… 
Os que saíram da lista: **Celestia**, **Parallax**, **ChIDE**…

Para textos curtos, frases, listas e anotações eu uso o Google Keep, textos maiores ou em desenvolvimento uso o Evernote. Como alternativa ao Evernote, há o ‘Paper’ do Dropbox (vale a penas dar uma olhada neste também).

## Mas e IDE? Um editor de código?

Caso seu ambiente utilize alguma plataforma que ofereça uma IDE prefira-o, sempre tenho comigo a ideia de ‘manter as coisas dentro de casa’ como exemplo usar o Eclipse Che estando na plataforma OpenShift da RedHaat ou usar o Cloud9 caso trabalhe com a AWS e tenha ‘livre acesso’ a usar um ambiente com o C9 (assim como sempre pergunto: para que usar um github/bitbucket/gitlab se você usa a AWS e lá temos o CodeCommit?).

Se você usa o Gitlab, vale a pena tentar usar o Web IDE dele, bem simples (até demais) porém funcional.

### IDE online para o Github

Caso seus projetos estejam todos no Github e você não fica offline, use o Gitpod!

Importante: A versão free limita a 100 horas por mês e repositórios públicos! A versão paga inclui tempo ilimitado, repositórios privados, suporte ao Gitlab dentre outros recursos.

Crie sua conta no [https://gitpod.io](https://gitpod.io) e dê as permissões para ele ler e escrever seus repositórios públicos.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017846361/HmaFRk3BA.png)

Pronto!

Agora basta acessar a pagina do repositório que deseja abrir com a IDE e na URL inserir o prefixo [**https://gitpod.io/#](https://gitpod.io/#)**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017848110/qC5mfiRik.png)

Usei como exemplo o versionamento da configuração do meu zsh.

Caso prefira, existe a extensão para o chrome que adiciona o botão na pagina do repositório:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017849798/LU0TUD2RV.png)

A plataforma irá subir um contêiner com o seu projeto aberto na IDE.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017851626/U9FjlmTSK.png)

Na pagina inicial do Gitpod você tem os workspaces que criou e consegue administra-los.
Possibilitando iniciar eles ou remove-los.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017853751/awOxg7h_H.png)

## IDE offline para Chromebook e ChromeOS

Sem dúvidas, o melhor app que testei foi o Code Pad Text Editor.

* É nativo na Chrome Store

* Funciona Offline

É possível usar seu sistema de controle de versão (desde que habilite o Linux no ChromeOS) e abrir os arquivos com o CodePad.

No meu exemplo usei o Git (costumo criar uma pasta chamada ‘Git’ e dentro dela colocar os projetos/repositórios).

Com o Linux habilitado no ChromeOS o terminal já terá instalado o Git e o SSH, então basta configurar as chaves ou clonar com https mesmo.

Obviamente, com o Linux habilitado no seu Chromebook (Debian!) nada te impede de instalar sua IDE preferida. Visual Code, Atom, Sublime, Gedit, Geany, Spyder, PyCharm, Notepad++, etc…

O Code Pad é open source, seu código-fonte está no github: [https://github.com/andrewbrg/codepad-chrome-app](https://github.com/andrewbrg/codepad-chrome-app)

Acesse-o na [Chrome Web Store](https://chrome.google.com/webstore/detail/code-pad-text-editor/adaepfiocmagdimjecpifghcgfjlfmkh?hl=pt-br&gl=br)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017855917/krrTYOG9Z.png)

O último commit no projeto foi a quase 1 ano atrás e no readme.md informa que está sem manutenção, porém é recente e pelo excelente app, espero logo um fork ou novas atualizações.

O CodePad suporta as seguintes linguagens: Bash, C/C++, CoffeeScript, CSS3, Dockerfile, GitIgnore, GoLang, HTML5, Java, JavaScript, JSON, Less, Markdown, MS SQL, Perl, PHP, PHTML, Plain text, Python, Ruby, Rust, Sass, Scala, SQL, TypeScript, XML, XHTML.

Barra lateral com navegação e gerenciamento do projeto

Diversos temas e configuração de fontes.

Syntax das linguagens, identificação e alertas de erros

Diversos snippets prontos e autocomplete para as linguagens suportadas.

Abaixo, abri o mesmo projeto que testei com o gitpod:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017857897/GK3hJRfa9.png)

A falta que senti é de integrar com o git para verificar os commits, diff, branch… ou alguma opção de integrar com plugins terceiros visto que ele edita e abre projetos a partir do gerenciador de arquivos do ChromeOS podemos ter projetos localmente, dropbox, Google Drive, NFS, diretórios compartilhados na rede… Porém, com algum projeto em git remoto (github, gitlab, bitbucket, etc…) ele não integra ainda (como a IDE do GitKraken por exemplo), a solução é habilitar o modo Linux do ChromeOS e baixar o repositório no seu dispositivo.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017859829/8HGE4G4cZ.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017861825/VB9tKxOM4.png)

Obviamente, com o Linux habilitado nada impede de instalar a IDE que mais gostei ou fique confortável do Linux em seu ChromeOS. Geany, Gedit, VSCode, Atom, Sublime etc…. o VI/VIM já está instalado nele =)
Mas fica aí essas duas dicas para você explorar mais estas opções.
