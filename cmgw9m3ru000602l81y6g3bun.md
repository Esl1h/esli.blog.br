---
title: "SRE e o Incident Commander"
datePublished: Sat Oct 18 2025 12:39:52 GMT+0000 (Coordinated Universal Time)
cuid: cmgw9m3ru000602l81y6g3bun
slug: sre-e-o-incident-commander
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/NhzhWMtk0E8/upload/1021341684e1e4a2e59845b0acae6021.jpeg
tags: sre, incident-management, incident-response, crisis, crisis-management, crises

---

O **Incident Commander** (IC) é uma figura crucial em situações de gerenciamento de incidentes, especialmente em ambientes de Site Reliability Engineering (SRE). O papel do Incident Commander está bem definido nos livros de SRE do Google e em práticas de engenharia de confiabilidade.

O Incident Commander é responsável por coordenar a resposta a incidentes em tempo real. Este papel é fundamental para garantir que a equipe se concentre na resolução do problema e na restauração do serviço o mais rápido possível.

O IC atua como o ponto central de comando e comunicação durante um incidente.

### Funções e Responsabilidades do Incident Commander

1. **Coordenação da Resposta ao Incidente**:
    
    * Organizar a equipe e alocar tarefas.
        
    * Manter o foco nas prioridades do incidente.
        
2. **Comunicação**:
    
    * Servir como o principal ponto de contato para todas as partes interessadas.
        
    * Fornecer atualizações regulares sobre o status do incidente.
        
3. **Tomada de Decisões**:
    
    * Tomar decisões rápidas e informadas para guiar a equipe.
        
    * Avaliar e priorizar diferentes abordagens para mitigar o incidente.
        
4. **Gerenciamento de Recursos**:
    
    * Garantir que a equipe tenha os recursos necessários para resolver o problema.
        
    * Mobilizar outras equipes ou especialistas, se necessário.
        
5. **Documentação**:
    
    * Manter um registro claro do que está acontecendo durante o incidente.
        
    * Documentar as decisões tomadas e as ações executadas.
        
6. **Pós-Incidente**:
    
    * Facilitar a revisão pós-incidente para aprender e melhorar processos futuros.
        
    * Identificar e implementar melhorias para evitar a recorrência do incidente.
        

### Características de um Bom Incident Commander

* **Experiência e Conhecimento Técnico**: O IC deve ter um conhecimento profundo da arquitetura e dos serviços da empresa.
    
* **Habilidades de Comunicação**: A capacidade de comunicar-se claramente e de maneira eficaz com a equipe e as partes interessadas é crucial.
    
* **Capacidade de Tomada de Decisões Sob Pressão**: O IC deve ser capaz de tomar decisões rápidas e eficazes em um ambiente de alta pressão.
    
* **Liderança**: Um bom IC deve ser capaz de liderar a equipe, inspirar confiança e manter a moral alta durante um incidente.
    
* **Orientação para a Resolução de Problemas**: O IC deve ser focado na resolução do incidente e na restauração dos serviços.
    

## Fluxo da atuação do Incident Commander/Manager e o SRE

Além das responsabilidades clássicas, equipes de SRE maduras operacionalizam o papel de IC com práticas que reduzem MTTx e evitam perda de contexto.

Um padrão é iniciar todo incidente com um briefing estruturado de 60–90 segundos (o “elevator pitch” do IC) contendo: o que está impactado, severidade, hipótese atual, próximos 1–2 passos, quem está em cada função (IC, Comms/Scribe, Tech Lead) e o próximo checkpoint de tempo.

Em seguida, o IC mantém um loop de controle cadenciado (por exemplo, a cada 10–15 min em SEV1): confirma objetivos, checa progresso das tarefas, explicita bloqueios, decide se pivota a estratégia, e delega comunicação para o Comms Lead com SLAs de atualização definidos (ex.: status page/execs a cada 30 min).

Esse ciclo evita “task thrashing” e garante alinhamento contínuo.

Em incidentes complexos, o IC aplica o “unified command”: mantém a autoridade de decisão, mas traz representantes de domínios críticos (segurança, banco de dados, rede, legal) para decisões rápidas, registrando todas as mudanças em um timeline único via ChatOps (Slack/Teams) com scribe dedicado, integrando alertas, mudanças e ações para reduzir handoffs e erros.

Do ponto de vista técnico, o IC deve orquestrar diagnóstico e mitigação com critérios explícitos de parada e reversão. Isso inclui:

**declarar** uma hipótese operacional por vez;  
**instrumentar** experimentos seguros (feature flags, toggles de tráfego, canary/partial rollbacks);  
**definir** “guardrails” (erro máximo, latência p95, taxa de 5xx) que, ao serem violados, disparam rollback sem nova deliberação;  
**separar** mitigação de causa-raiz (restaura-se primeiro, investiga-se depois).

Métricas e dados são centrais: o IC orienta a equipe a trabalhar sobre sinais de observabilidade (logs estruturados, traces, métricas de saturação) e dependências mapeadas (CMDB/Service Graph) para identificar blast radius e priorizar serviços de maior valor.

Ao encerrar, conduz um debrief curto “hot wash” para capturar follow-ups enquanto o contexto ainda está fresco e agenda um postmortem com ações rastreáveis (due date/owner), garantindo aprendizado organizacional e evolução do runbook e das automações (por exemplo, criar workflows no PagerDuty/Opsgenie para auto-atribuir papéis, abrir bridges e postar updates padrão).