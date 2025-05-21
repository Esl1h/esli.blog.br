---
title: "As documentações do SRE"
seoTitle: "Guia de Documentação para SRE"
seoDescription: "Explore documentações essenciais e melhores práticas para SREs para otimizar processos operacionais"
datePublished: Sun Sep 18 2022 15:57:00 GMT+0000 (Coordinated Universal Time)
cuid: cl87iuf6g08mcwhnvdz3fbgl0
slug: as-documentacoes-do-sre
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/Hzp-1ua8DVE/upload/v1663516244506/sCA0UmLLu.jpeg
tags: sysadmin, books, documentation, sre, policy

---

Esta lista com as documentações essenciais para os SREs faz parte do artigo ["DocOps + Doc as a Code"](https://esli.blog.br/doc-as-a-code-docops-sre), decidi separar num post único ;-)

Os SREs são solucionadores de problemas e desenvolvedores!

Eles geralmente dividem seu tempo igualmente entre o desenvolvimento de software para melhor desempenho e disponibilidade do site + operações de TI e tarefas de suporte.

As funções e responsabilidades críticas do SRE incluem:

* Desenvolver o software e os processos necessários para manter os serviços. As ferramentas desenvolvidas para manter os serviços normalmente envolvem coleta de dados e monitoramento extensivo.
    
* Capture e analise as principais métricas, como disponibilidade, tempo médio entre falhas e tempo médio de reparo, e desenvolva novas métricas e KPIs conforme necessário. Adicione essas métricas aos painéis de monitoramento e sistemas de relatórios.
    
* Use o monitoramento detalhado para melhorar a disponibilidade e o desempenho de aplicativos, serviços, sistemas e infraestrutura. Crie novos alertas para encontrar anomalias e entender a causa raiz das falhas do sistema.
    
* Crie e implante arquiteturas de automação, alertas, autorrecuperação e outras tecnologias para tornar o ambiente mais sustentável.
    
* Monitore, gerencie e solucione problemas de processos regulares para melhorar processos e fluxos de trabalho.
    
* **Criar e manter documentação de processos, automação, infraestrutura, recursos e serviços.**
    
* Atuar como especialista no assunto e treinador para orientar desenvolvedores e engenheiros, bem como auxiliar desenvolvedores juniores na solução de problemas e depuração de software.
    

## Documentação fundamental do SRE

Alguns tipos de documentação comuns são fundamentais para a função SRE e as melhores práticas operacionais. Estas abaixo são as principais, essenciais, básicas e obrigatórias para qualquer SRE:

* PRR
    
* PostMortem
    
* Service overview
    
* Playbooks
    
* Políticas
    

## PRR - Product readiness review documentation

Quando os SREs integram novos sistemas, eles realizam uma revisão de prontidão de produção (Product readiness review documentation) para garantir que os novos sistemas atendam aos padrões de prontidão de sua organização.

Escrever um PRR exige que as equipes de TI sejam o mais descritivas possível ao documentar a prontidão do sistema.

---

## PostMortem

Um documento postmortem típico inclui o seguinte:

um resumo de gerenciamento capturando os efeitos e a causa raiz do incidente;

um resumo técnico dos efeitos do incidente nos negócios, usuários, equipes e sistemas, incluindo tempos de resposta aproximados, método de detecção e a solução que a equipe de SRE aplicou para resolver o incidente;

histórico de incidentes com detalhes adicionais de detecção e capturas de tela de gráficos de monitoramento, linha do tempo, causas-raiz e informações de resolução; e

lições aprendidas, incluindo detalhes sobre o que correu bem e o que correu mal durante a resolução de incidentes.

---

## Service Overview

Um documento de visão geral do serviço.

Deve permitir entender a arquitetura do sistema, componentes, dependências e contratos de serviço para cada serviço que eles suportam. Consequentemente, as visões gerais de serviço são documentos críticos de alta prioridade. Os elementos comuns de uma visão geral do serviço incluem o seguinte:

Descrição do Serviço;

links para outras fontes de informação, como painéis de monitoramento e documentação de operações; e

arquitetura de referência.

---

## Playbooks/Runbooks

Documentações de operações básicas que permitem que os engenheiros de plantão respondam aos alertas de monitoramento de serviço.

Um runbook bem elaborado e mantido reduz o tempo necessário para mitigar um incidente, pois contém procedimentos de solução de problemas e links para operações e consoles de monitoramento.

Os elementos comuns de um playbook SRE incluem o seguinte:

* definição de um incidente para sua organização;
    
* designação de funções e responsabilidades de resposta a incidentes;
    
* procedimentos e fluxos de trabalho padronizados de resposta a incidentes, revisados ​​e testados por um SRE; e
    
* Documentos de dicas e listas de verificação para resposta a incidentes de SRE.
    

As práticas recomendadas de criação de playbooks incluem o seguinte:

* Comece cada manual com um gatilho, como um alerta de monitoramento.
    
* Estruture as entradas do manual com base na gravidade, impacto, métrica, histórico, mitigação e descoberta.
    
* Automatize todas as ações - incluindo etapas simples - para remover o máximo possível de erros humanos do processo de resposta a incidentes.
    

---

## Políticas

A operação de sistemas complexos em larga escala exige políticas técnicas e não técnicas para a produção.

A documentação da política detalha todos os mandatos para tarefas de produção, como log de alterações, retenção de log, nomeação de serviço interno, acesso e uso de credenciais de emergência, backups, etc...

Links: https://www.techtarget.com/searchitoperations/tip/An-introduction-to-SRE-documentation-best-practices