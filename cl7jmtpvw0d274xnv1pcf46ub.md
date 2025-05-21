---
title: "Doc as a Code, DocOps e o SRE"
datePublished: Thu Sep 01 2022 22:41:57 GMT+0000 (Coordinated Universal Time)
cuid: cl7jmtpvw0d274xnv1pcf46ub
slug: doc-as-a-code-docops-sre
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/59yRYIHWtzY/upload/v1662072021304/Nl43ZybRk.jpeg
tags: markdown, documentation, sre, docs, asciidoctor

---

# Doc as a Code

Documentation as Code (Docs as Code) refere-se a uma filosofia em que você deve escrever a documentação com as mesmas ferramentas que escreveu seu código:
- Issue Trackers
- Version Control (Git)
- Plain Text Markup (Markdown, reStructuredText, Asciidoc)
- Code Reviews
- Automated Tests


Isso significa seguir os mesmos fluxos de trabalho das equipes de desenvolvimento e ser integrado à equipe do produto. 

Ele permite uma cultura em que escritores e desenvolvedores se sintam proprietários da documentação e trabalhem juntos para torná-la o melhor possível.


A má documentação reflete mal na qualidade do produto e da sua empresa. Uma parte fundamental da abordagem Docs As Code é aplicar um modelo de Garantia de Qualidade (QA) em seu processo de documentação. 

Isso significa executar testes de documentação da mesma maneira que os desenvolvedores executam testes automatizados no software que estão escrevendo.
O teste de documentação é um teste não funcional de seu conteúdo que ajuda a garantir que ele permaneça atualizado com seu produto, os requisitos de instalação e seja compreensível para seu público. 


Exemplo simples 1:

Skubber - Client para a API do Kubernetes feito em Scala

Site: https://skuber.co/

Escrito em Markdown, versionado no próprio app, gerado com Docsify e hospedado no github.

https://github.com/hagay3/skuber/tree/master/docs

Code: https://github.com/hagay3/skuber


Abaixo, a apresentação de Marcia Riefer Johnston e Dave May sobre a mudança na AWS para o **docs as code** na conferência virtual **Write the Docs** Portland 2022.

%[https://youtu.be/Cxuo3udElcE]

Obs.: * **Write the Docs** é uma comunidade global de pessoas que se preocupam com documentação.*

Links:
https://pronovix.com/blog/docops-engineering-stunning-api-documentation


Livro “Modern Technical Writing: An Introduction to Software Documentation” https://www.amazon.com/Modern-Technical-Writing-Introduction-Documentation-ebook/dp/B01A2QL9SS



# DocOps

Para manter a competitividade nos negócios, agilidade é essencial.

Equipes de desenvolvimento de software migraram para metodologias de desenvolvimento Agile e DevOps


Mas suas documentações se mantiveram na Idade das Pedras. Ainda em cascata, sendo uma etapa a ser entregue no final (e muitas vezes já nasce desatualizada) e quase sempre, somado a isto, elas sofrem da sindrome ROT.

**Síndrome do Conteúdo ROT - Redundant, Outdated and Trivial**

Com Agilidade não há tempo para a pausa e realizar a 'transferência' do conhecimento para uma documentação que não entrou na mudança cultural. Não há colaboração, integração e entrega contínua neste ponto.

O DocOps trata do estabelecimento de documentação contínua para tornar o DevOps (entrega contínua) bem-sucedido. Isso exigirá a integração de departamentos e usuários de equipes interdisciplinares no processo de criação de documentos. Idealmente, uma nova função de 'Gerente de Informações' deve: 

- Ser responsável por todos os requisitos de documentos
- Controlar a criação e manutenção de informações 
- Disponibilizar informações através de um portal padronizado.

O DocOps trata da criação de uma Cadeia de Suprimentos de Conteúdo que seja colaborativa, ágil e contínua em sua essência.

Com o DocOps é possível zelar, encarregar e tornar-se mais responsável pelo conteúdo de autores internos e externos durante todo o ciclo de vida do produto. 

O conteúdo é então publicado usando uma plataforma que fornece uma única fonte de conteúdo contextualmente vinculada às interfaces do produto. 

E permite que a empresa obtenha feedback operacional contínuo e faça alterações em tempo real para melhorar a experiência do cliente.

Alguns princípios-chave por trás do que o DocOps faz:


- Cria um fluxo contínuo de documentação técnica, semelhante ao modo como a integração contínua cria um fluxo contínuo para desenvolvedores de software.

- Move todos os documentos para uma plataforma colaborativa baseada em nuvem, como um wiki, que se torna a principal plataforma de publicação e entrega de documentação do usuário.

- Alinha ferramentas Agile comuns com uma plataforma de publicação de documentação.

O DocOps pede para aplicar análises e relatórios para monitorar qual conteúdo está tendo um desempenho melhor do que outros. 

À medida que o gerenciamento e a empresa se tornam mais orientados por dados, permite o gerenciamento melhor do conteúdo e concentram seus esforços de desenvolvimento no conteúdo que os leitores precisam versus o que a equipe de documentação percebe como necessidades dos leitores.

O DocOps é essencial para permitir agilidade nos negócios!

DocOps - How CA Technologies Got Started!
https://www.linkedin.com/pulse/20140911020510-10335988-the-ca-technologies-docops-platform-phase-1/


Case da CA Technologies:
- +300 Devs
- +24.000 Documentos (de 100 a 1000 páginas cada)
- +300 produtos
- 19 idiomas

Ferramentas utilizadas: Atlassian + Appfusions + Lingotek

https://lingotek.com/images/pdf/Lingotek_Case_Study_CA_04_11_12.pdf 

Outros exemplos:

Muitas empresas utilizam a plataforma https://readme.io 

O pessoal da [Stripe](https://stripe.com/docs) utiliza a plataforma https://markdoc.dev, desenvolvida por eles mesmos e disponibilizada open source.




# SRE + DocOps/DocAsCode

**As documentações principais do SRE**

Os SREs são solucionadores de problemas e desenvolvedores. 


Eles geralmente dividem seu tempo igualmente entre o desenvolvimento de software para melhor desempenho e disponibilidade do site + operações de TI e tarefas de suporte.

As funções e responsabilidades críticas do SRE incluem:
- Desenvolver o software e os processos necessários para manter os serviços. As ferramentas desenvolvidas para manter os serviços normalmente envolvem coleta de dados e monitoramento extensivo.
- Capture e analise as principais métricas, como disponibilidade, tempo médio entre falhas e tempo médio de reparo, e desenvolva novas métricas e KPIs conforme necessário. Adicione essas métricas aos painéis de monitoramento e sistemas de relatórios.
- Use o monitoramento detalhado para melhorar a disponibilidade e o desempenho de aplicativos, serviços, sistemas e infraestrutura. Crie novos alertas para encontrar anomalias e entender a causa raiz das falhas do sistema.
- Crie e implante arquiteturas de automação, alertas, autorrecuperação e outras tecnologias para tornar o ambiente mais sustentável.
- Monitore, gerencie e solucione problemas de processos regulares para melhorar processos e fluxos de trabalho.
- **Criar e manter documentação de processos, automação, infraestrutura, recursos e serviços.**
- Atuar como especialista no assunto e treinador para orientar desenvolvedores e engenheiros, bem como auxiliar desenvolvedores juniores na solução de problemas e depuração de software.


### Documentação fundamental do SRE

Alguns tipos de documentação comuns são fundamentais para a função SRE e as melhores práticas operacionais. Estas abaixo são as principais, essenciais, básicas e obrigatórias para qualquer SRE:

- PRR
- PostMortem
- Service overview
- Playbooks
- Políticas



### PRR - Product readiness review documentation

Quando os SREs integram novos sistemas, eles realizam uma revisão de prontidão de produção (Product readiness review documentation) para garantir que os novos sistemas atendam aos padrões de prontidão de sua organização.

Escrever um PRR exige que as equipes de TI sejam o mais descritivas possível ao documentar a prontidão do sistema.



### PostMortem

Um documento postmortem típico inclui o seguinte:

um resumo de gerenciamento capturando os efeitos e a causa raiz do incidente;

um resumo técnico dos efeitos do incidente nos negócios, usuários, equipes e sistemas, incluindo tempos de resposta aproximados, método de detecção e a solução que a equipe de SRE aplicou para resolver o incidente;

histórico de incidentes com detalhes adicionais de detecção e capturas de tela de gráficos de monitoramento, linha do tempo, causas-raiz e informações de resolução; e

lições aprendidas, incluindo detalhes sobre o que correu bem e o que correu mal durante a resolução de incidentes.



### Service Overview

Um documento de visão geral do serviço. 

Deve permitir entender a arquitetura do sistema, componentes, dependências e contratos de serviço para cada serviço que eles suportam. Consequentemente, as visões gerais de serviço são documentos críticos de alta prioridade. Os elementos comuns de uma visão geral do serviço incluem o seguinte:

Descrição do Serviço;

links para outras fontes de informação, como painéis de monitoramento e documentação de operações; e

arquitetura de referência.


### Playbooks/Runbooks

Documentações de operações básicas que permitem que os engenheiros de plantão respondam aos alertas de monitoramento de serviço. 

Um runbook bem elaborado e mantido reduz o tempo necessário para mitigar um incidente, pois contém procedimentos de solução de problemas e links para operações e consoles de monitoramento.

Os elementos comuns de um playbook SRE incluem o seguinte:

- definição de um incidente para sua organização;
- designação de funções e responsabilidades de resposta a incidentes;
- procedimentos e fluxos de trabalho padronizados de resposta a incidentes, revisados ​​e testados por um SRE; e 
- Documentos de dicas e listas de verificação para resposta a incidentes de SRE.


As práticas recomendadas de criação de playbooks incluem o seguinte:

- Comece cada manual com um gatilho, como um alerta de monitoramento.
- Estruture as entradas do manual com base na gravidade, impacto, métrica, histórico, mitigação e descoberta.
- Automatize todas as ações - incluindo etapas simples - para remover o máximo possível de erros humanos do processo de resposta a incidentes.



### Políticas

A operação de sistemas complexos em larga escala exige políticas técnicas e não técnicas para a produção. 

A documentação da política detalha todos os mandatos para tarefas de produção, como log de alterações, retenção de log, nomeação de serviço interno, acesso e uso de credenciais de emergência, backups, etc...



Links:
https://www.techtarget.com/searchitoperations/tip/An-introduction-to-SRE-documentation-best-practices

