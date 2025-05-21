---
title: "Melhores Praticas para SRE"
datePublished: Sun Aug 29 2021 18:00:57 GMT+0000 (Coordinated Universal Time)
cuid: cksxiqveb0oi3kxs18qei40cu
slug: melhores-praticas-para-sre
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1630259987470/VliU_rkRP.jpeg
tags: sysadmin, devops, software-engineering

---

O que é Site Reliability Engineering (SRE)?

Se esta pergunta precisa ser respondida ou esclarecida para você, veja este artigo:

%[https://esli.blog.br/podcast-sobre-sysadmin-e-sre-briefing-e-ep01-sre-devops-e-o-sysadmin-raiz]


Ou escute-o como podcast, disponível em diversas [plataformas](https://anchor.fm/Esl1h)!  
  
youtube:
%[https://www.youtube.com/watch?v=dEpuuJc6JMU]

anchor:
<iframe src="https://anchor.fm/esl1h/embed/episodes/Podcast-Sysadmin-01-SRE--DevOps-e-o-Sysadmin-Raiz-eh9dr5" height="102px" width="400px" frameborder="0" scrolling="no"></iframe>


Enfim...
O conceito de engenharia de confiabilidade de site (SRE) se originou no Google. A ideia está intimamente relacionada aos princípios do DevOps. É uma abordagem às operações de TI (novamente, aconselho o artigo acima, mais abrangente e menos simplista em sua definição, além de comparações do SRE com sysadmin, devops, ops, ...).

As equipes de SRE assumem as tarefas manuais das equipes de operações, e as entregam a engenheiros que usam ferramentas e automação para resolver problemas e gerenciar sistemas de produção.

É uma prática valiosa ao criar sistemas escalonáveis ​​e altamente confiáveis. Ele ajuda as organizações a gerenciar uma infraestrutura massiva por meio de código, que é mais escalonável e sustentável para administradores de sistema que gerenciam centenas de milhares de máquinas.


## Por que precisamos de SRE? É importante? E o que torna uma boa equipe SRE?

O SRE atua como uma ponte entre a engenharia de software e as operações de TI e preenche a lacuna entre elas. Em quase todos os lugares, o SRE entra em ação quando se trata de preparação para falhas em sistemas de produção. Ele garante que os sistemas da organização sejam escalonáveis, confiáveis, previsíveis e automatizados.

O SRE também define Indicadores de Nível de Serviço (SLIs), Objetivos de Nível de Serviço (SLOs), Acordo de Nível de Serviço (SLA) que define os números reais de desempenho, os objetivos que sua equipe deve atingir para cumprir esse acordo e quão confiáveis ​​os sistemas precisam ser para os usuários finais.

O objetivo principal do SRE é melhorar o desempenho e a eficiência operacional.

Portanto, um SRE não é apenas "uma pessoa de operações que codifica". 

Em vez disso, o SRE é outro membro da equipe de desenvolvimento com um conjunto diferente de habilidades, particularmente em torno de implantação, gerenciamento de configuração, monitoramento, métricas, etc. Assim como um engenheiro que desenvolve uma boa aparência para um aplicativo, ele deve saber como os dados são buscados de um armazenamento de dados, um SRE não é o único responsável por essas áreas. Toda a equipe trabalha em conjunto para entregar um produto que pode ser facilmente atualizado, gerenciado e monitorado. A necessidade de um engenheiro de confiabilidade de site surge naturalmente quando uma equipe está implementando DevOps, mas percebe que está exigindo muito dos desenvolvedores e precisa de um especialista para saber o que a equipe de operações costumava fazer.

## SRE vs DevOps e como o SRE funciona com DevOps?

Em sua essência, a engenharia de confiabilidade do site é uma implementação do paradigma DevOps. Assim como a integração e entrega contínuas (CI / CD) são aplicações dos princípios do DevOps para o lançamento de software, o SRE é uma aplicação desses mesmos princípios para a confiabilidade do software.

Há uma grande variedade de maneiras de definir DevOps. Ainda assim, o modelo tradicional é onde as equipes de desenvolvimento (“devs”) e operações (“ops”) são separadas, fazendo com que a equipe que escreve o código não seja responsável por como funciona quando os clientes começam a usá-lo. A equipe de desenvolvimento “jogaria o código por cima da parede” para a equipe de operações instalar e dar suporte.

De acordo com a abordagem do Google, você pode usar o SRE para adotar melhor os princípios do DevOps na organização e medir o sucesso de sua implementação.

Para entender melhor como combinar os dois, considere os seguintes princípios:


> 

- **Reduza os silos organizacionais:** o SRE ajuda ao compartilhar a propriedade entre desenvolvedores e equipes de operações. Este é um dos princípios básicos de uma filosofia DevOps. Quando os SREs se concentram em melhorar a detecção de problemas e o desempenho dos aplicativos, as equipes de operações podem se concentrar no gerenciamento da infraestrutura e os desenvolvedores podem se concentrar nas melhorias de recursos.
- **Aceite a falha normalmente:** como o DevOps, os SREs não passam a culpa por falhas e incidentes de produção entre as equipes de TI. As autópsias sem culpa são uma prática recomendada de SRE que garante que todos os incidentes sejam usados ​​como oportunidades de aprendizagem. Quando a possibilidade de falha é normalizada, as equipes podem assumir riscos mais significativos, potencialmente levando a maiores inovações sem medo de contratempos excessivos ou tempo de inatividade.
- **Implementar mudanças graduais:** como DevOps, SRE também incentiva a melhoria contínua por meio de mudanças. SRE requer que as mudanças sejam pequenas e frequentes. Como resultado, quaisquer repercussões negativas são menos impactantes e as melhorias de baixo risco podem ser prontamente testadas e implementadas.
- **Aproveitando ferramentas e automação:** enquanto o DevOps incentiva a automação e a adoção de tecnologia, o SRE está focado em adotar tecnologias consistentes e acesso a informações entre as equipes de TI. Isso facilita o gerenciamento de operações e reduz a chance de problemas criados por incompatibilidades tecnológicas. Essa padronização também ajuda a garantir que os membros de uma equipe possam colaborar melhor, já que as ferramentas são uniformes e têm menos probabilidade de exigir conjuntos de habilidades especializadas que alguns membros não têm.
- **Meça tudo:** o SRE combina métricas com ciclos de feedback para medir as operações e identificar oportunidades de melhoria. Ele também aumenta a folga para operações manuais e de risco conforme necessário, tornando-o mais previsível por meio da medição. Ao aplicar dados de métricas, as equipes podem definir metas adequadas, mantendo expectativas razoáveis ​​de desempenho.
Agora que sabemos por que o SRE é importante, vamos passar para as melhores práticas de SRE que você deve seguir ao adotar a cultura SRE.




## Melhores Práticas SRE

Ao implementar o SRE, pode levar algum tempo para refinar sua estratégia e personalizar as práticas para atender às suas necessidades operacionais. Para ajudar a acelerar esse processo, considere os seguintes princípios e melhores práticas de SRE.

-  Definir o quão você poderá falhar, seu orçamento de erro.

Em suma, um orçamento de erro é a quantidade de erros que seu serviço pode acumular ao longo de um determinado período de tempo antes que seus usuários comecem a ficar insatisfeitos. Você pode pensar nisso como a tolerância à dor para seus usuários, mas aplicada a uma dimensão específica de seu serviço: disponibilidade, latência e assim por diante. 
Definir seu SLO como um usuário. Com ele definido, para achar seu orçamento de erro, subtraia o SLO de 100 (os 100%). O resultado será seu "espaço" de erro.
Eles também são uma oportunidade para as equipes de desenvolvimento inovarem e assumirem riscos.

-  Falhe de forma sensata/controlada.

Sanitize e valide as entradas de configuração e responda aos dados improváveis de ambos, permitindo a continuidade da operação num estado anterior e alertando para o recebimento dessa entrada ruim. 
No livro 'Clean Code', o sanitizar dados é bem abordado, mas infelizmente, muitos não dão importância e confiam demasiadamente no input que seu app receberá.

-  Monitoramento de erros e disponibilidade

Para identificar erros de desempenho e manter a disponibilidade do serviço, as equipes de SRE precisam ver o que está acontecendo em seus sistemas. O monitoramento é necessário para verificar se um aplicativo / sistema está se comportando conforme o esperado. Isso significa um serviço que atende a objetivos específicos e compreende o que acontece quando uma mudança é feita. Além disso, queremos saber antes do cliente.
O monitoramento vai gerar alguns tipos de saídas: exibição numa pagina, vai criar um ou gerar um log.
Se for importante o suficiente para incomodar um humano, deve requerer ação imediata ou ser tratado como um bug e inserido em seu sistema de rastreamento de bugs. 

-  Capacidade de planejamento eficiente

As organizações precisam planejar coisas como crescimento orgânico, que pode ser maior adoção de produtos, crescimento inorgânico, que vem de saltos repentinos na demanda devido a lançamentos de recursos, campanhas de marketing, etc. Isso consumirá mais recursos (como interrupções na Black Friday ou Cyber Monday). Para se preparar para esses eventos, você precisará prever a demanda e planejar o tempo de aquisição.
Facetas importantes do planejamento de capacidade incluem testes de carga regulares e provisionamento preciso. O teste de carga regular permite que você veja como o seu sistema opera sob a tensão média dos usuários diários. Além disso, adicionar capacidade de qualquer forma pode ser caro, portanto, saber onde você precisa de recursos adicionais é a chave.

-  Prestando atenção ao gerenciamento de mudanças

Em muitas organizações, a maioria das interrupções é causada por alterações em um sistema ativo, seja para um novo envio binário ou um novo envio de configuração. Cada pequena mudança impacta o negócio. Portanto, analise cada mudança para o risco que acarreta. Deve ser supervisionado. Considere o impacto das mudanças de longo curso tendo uma visão geral, não apenas como elas podem afetar o sistema hoje.
Para garantir que nada inesperado ocorra durante a mudança, ela deve ser monitorada pelo engenheiro que executa o estágio de implantação ou, de preferência, por um sistema de monitoramento comprovadamente confiável. Se um comportamento inesperado for detectado, reverta primeiro e diagnostique depois para minimizar o tempo médio de recuperação (MTTR).

-  Post-mortem sem culpados (blameless), aprender com o fracasso.

Uma cultura post mortem verdadeiramente inocente ajuda a construir um sistema mais confiável nas organizações. Deve ser isenta de culpa e focar no processo e na tecnologia, não nas pessoas.
Suponha que as pessoas envolvidas em um incidente sejam inteligentes, bem-intencionadas e estivessem fazendo as melhores escolhas que podiam com as informações de que dispunham no momento. Atribuir um incidente a uma pessoa ou grupo de pessoas é contraproducente. Isso cria um ambiente onde as pessoas têm medo de correr riscos, inovar e resolver problemas.
Falhas acontecerão. Não há como contornar isso. Mas, por ter uma boa resolução de incidentes e prática retrospectiva em vigor, as falhas podem ser benéficas. Ele descobre áreas nas quais se concentrar para melhorar a resiliência. Contanto que você aprenda com um incidente, você progrediu.

- Gestão de Trabalho

Um dos principais focos do SRE é a automação. O trabalho árduo é uma perda de tempo precioso de engenharia e, com a criação de estruturas, processos, ferramentas internas / ferramentas de construção de SREs para eliminá-lo, os engenheiros podem voltar a inovar.
As equipes de SRE não devem dedicar mais do que 50% de seu tempo no trabalho operacional, o estouro operacional deve ser direcionado à equipe de desenvolvimento de produto. Muitos serviços também incluem os desenvolvedores de produtos na rotação do plantão e no gerenciamento de tickets, mesmo que não haja estouro no momento. Isso gera incentivos para projetar sistemas que minimizam ou eliminam o trabalho árduo operacional, além de garantir que os desenvolvedores de produtos estejam em contato com o lado operacional do serviço. Uma reunião regular de produção entre SREs e a equipe de desenvolvimento também é útil.
Ironicamente, se você implementar essas práticas recomendadas, a equipe SRE pode acabar ficando sem prática em responder a incidentes devido à sua pouca frequência, tornando uma interrupção mais longa do que realmente deveria. Pratique o tratamento de interrupções hipotéticas (Disaster Role Playing) rotineiramente e aprimore sua documentação de tratamento de incidentes no processo.


Claro, estes itens acima são apenas o principio!
Se estiver começando agora, só tenho uma dica: leia o livro SRE, visite o site e passe pelos demais artigos %[https://sre.google/]

Referências:

https://www.infracloud.io/blogs/sre-best-practices/

https://sre.google/sre-book/service-best-practices/
