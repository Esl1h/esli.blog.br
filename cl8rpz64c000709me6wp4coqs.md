---
title: "Docs-as-Code: Documentação como código"
datePublished: Sun Oct 02 2022 19:12:02 GMT+0000 (Coordinated Universal Time)
cuid: cl8rpz64c000709me6wp4coqs
slug: docs-as-code-documentacao-como-codigo
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/cwuSKXhwhxI/upload/v1664737786814/g0i1eN_XW.jpeg
tags: markdown, documentation, agile, project-management, docs

---

No post anterior fiz uma introdução sobre o [Docs-as-Code](https://esli.blog.br/doc-as-a-code-docops-sre#heading-doc-as-a-code) e também sobre o [DocOps](https://esli.blog.br/doc-as-a-code-docops-sre#heading-docops).

Confere lá e vamos aprofundar um pouco mais aqui...

Em resumo, Documentation as Code (Docs as Code) refere-se a uma filosofia em que você deve escrever a documentação com as mesmas ferramentas que escreveu seu código, ou seja, monitorar o trabalho/ideias/problemas atravês de 'issues' com o jira, Gitea, gitlab, github, bugzilla, redmine ou outros... usar versionamento (git), a documentação escrita (markdown, asciidoc, latex...) e usar esteiras de validações e testes automaticos.

Portanto, significa seguir os mesmos fluxos de trabalho das equipes de desenvolvimento e ser integrado à equipe do produto.


Quantas vezes ocorreu estar em desenvolvendo um trabalho e ao olhar sua documentação, mesmo não concluido ainda, ela já tornou-se redundante, desatualizada e sem valor significante para o trabalho.


Mesmo com diversas metodologias, scrum, kanban, diversos sistemas web bonitos para gerenciar e rastrear o trabalho sendo feito, dentro do time ou da corporação existe alguma preocupação em relação a documentação? Há um pensamento sobre mante-la sempre em constante evolução e construção na mesma proporção em que códigos, ferramentas e features são entregues?


Essa preocupação (se existir!) é devido alguém especifico no time que levanta a bandeira e torna-se o seu único defensor ou é uma preocupação e parte do fluxo em que todos entender ser algo primordial para a entrega do trabalho?


Quando trabalhou em uma documentação, quanto tempo levou para que ela torna-se obsoleta?

Não importa qual nome bonito utiliza em seu dia-a-dia do trabalho, se estão usando métodos ágeis, DevOps, Engenharia, CI/CD, automações... todo o desenvolvimento mudou para incluir a melhoria continua em todos os seus passos, desde o desenvolvimento, esteiras, entrega, infraestrutura e etc...

- **Mas a documentação?**

A documentação ainda é tratada no modelo cascata. Em alguma parte do trabalho, haverá uma tarefa de documentação e neste antigo modelo de desenvolvimento de software sequencial, o processo é visto como um fluir constante para frente... daquele ponto da documentação em diante, tudo já será inútil. Pois a documentação foi escrita e entregue em um ponto na linha do tempo.

Aparentemente, muitos lugares leram o manifesto ágil e em "Software em funcionamento mais que documentação abrangente" entenderam "software em funcionamento sem documentação" (*Working software over comprehensive documentation* IS NOT/DON'T MEAN *Working software over documentation*)

- **Como resolver?**

Não há. 
Não há ferramenta, não existe sistema ou solução out-of-box.
Lembre-se: Indivíduos e interações mais que processos e ferramentas.

Adotar uma nova ferramenta brilhante sem abordar as pessoas e o elemento do processo dificilmente produzirá qualquer melhoria.

Já trabalhei com processos em arquivos .doc separados em pastas. Em outros lugares usei wikis (como a MediaWiki, e em outra empresa já inclusive implementei o DokuWiki - tema para outro post!!!), sistemas opensources como Redmine, proprietários como o Jira, outros complexos como o Asana e simples como o Trello. Nenhum deles irá correr 100% sem pessoas (as que fazem parte do processo) adotarem realmente.

Abaixo uma parte do artigo do `@yam-yam-architect` para o https://betterprogramming.pub.

=====



### Pessoas e Processos: Pré-requisitos

> - Coesão da equipe: as equipes devem estar alinhadas a um objetivo comum e ser capazes de se comunicar de forma eficaz. Inicialmente, pode ser prudente manter uma agenda regular para garantir que a iniciativa permaneça no curso.
> - Identifique os editores de conteúdo para cada área de informação – exemplos: integração do desenvolvedor, catálogo de API, dados.
> - Definir e concordar com um processo leve; evite fazer isso isoladamente para garantir que a equipe esteja de acordo com as novas formas de trabalhar.
> - Defina modelos, diretrizes e uma taxonomia simples.
> - Integrar ou (se possível) alterar os processos existentes. 





Adotando uma mentalidade de engenharia de software

Antes de ir ao mercado em busca de uma nova ferramenta de gestão do conhecimento, por que não avaliar os investimentos que você já fez? Uma cadeia de ferramentas de DevOps pode fornecer tudo o que você precisa: autoria de conteúdo usando markdown, fluxos de trabalho de aprovação usando pull-requests GIT, controle de qualidade usando pipelines e um visualizador de conteúdo pronto para uso.

Você pode publicar o conteúdo em um site estático. Muitos fornecedores de produtos fazem isso para sua documentação externa. Por exemplo, versionando o conteudo em markdown no github e utilizando o Github pages para gerar um site estático e apontar o subdominio da documentação para lá. A documentação entra nos processos de Workflow/GitFlow assim como o codigo-fonte do produto/ferramenta/aplicação inclusive até sua entrega com as etapas de CI/CD, teste e validações... enfim, tudo.




### Visão geral

O autor cria uma branch local para suas novas alterações de conteúdo. 

Eles usam um IDE de sua escolha, por exemplo, VSCode, para criar arquivos markdown. O VSCode também inclui extensões para ferramentas de diagramação como o DrawIO, para que você possa até controlar a versão de seus diagramas!


O autor publica a ramificação no sistema de gerenciamento de controle de origem. Tecnicamente, eles não precisariam fazer isso o tempo todo, mas é uma boa prática, pois fornece um backup.

O autor cria um Pull Request (PR) quando estiver pronto para enviar suas alterações de conteúdo para aprovação.

Os editores de conteúdo receberão uma notificação informando-os de um novo PR. O editor revisará o conteúdo e decidirá aprovar ou rejeitar a submissão; eles podem adicionar comentários ao conteúdo novo/alterado para que o autor saiba o que é necessário para aprovação.

Depois que um PR é criado/atualizado (após o feedback), um pipeline automatizado executará várias verificações de qualidade em relação ao conteúdo.


Uma vez que o editor tenha aprovado o PR, o conteúdo pode ser mesclado no ramo principal.


O conteúdo da ramificação principal pode ser exibido aos usuários que usam a plataforma DevOps, por exemplo, Azure DevOps WIKI.


![docs-as-code.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1664737262464/JN6AwL82I.png align="left")



Pensamentos finais
Criar e gerenciar documentação de qualidade não é inviável. O maior desafio é abordar o elemento de pessoas e processos.

A maioria das pessoas não vê o valor na documentação, então você precisa vender os benefícios e colocar as equipes a bordo.

Defina e, sempre que possível, automatize o processo - isso pode incluir desde aprovação; verificações de qualidade automatizadas, como ortografia e gramática; até a publicação de conteúdo externo, como WIKIs e portais de desenvolvedores.

Como em qualquer sistema, comece simples e adicione/altere a funcionalidade ao longo do tempo.