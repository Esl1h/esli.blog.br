---
title: "SRE e o gerenciamento de crises e incidentes"
datePublished: Thu Oct 16 2025 12:21:57 GMT+0000 (Coordinated Universal Time)
cuid: cmgte3ckk000702ld6jmmd8cd
slug: sre-e-o-gerenciamento-de-crises-e-incidentes
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/tEIHSmfwznM/upload/509cfc2a610f050bde61d054bf22284b.jpeg
tags: sre, incident-management, incident-response, crisis, crises

---

O papel de Site Reliability Engineering (SRE) em crises e incidentes é reduzir o tempo para restaurar serviço com segurança, minimizar impacto ao cliente e aprender sistematicamente com cada ocorrência. Isso exige preparação (processos e runbooks), resposta coordenada (comando e comunicação), execução técnica disciplinada (diagnóstico, mitigação e rollback) e melhoria contínua (postmortems sem culpa e correções estruturais).

SRE eficaz em crises e incidentes é menos sobre “heróis” e mais sobre sistemas e práticas que tornam o comportamento correto o caminho mais fácil. Com SLOs claros, processos de incidente bem definidos, comunicação disciplinada e aprendizado contínuo, sua organização reduz impacto ao usuário e acelera a recuperação. Faça da preparação um hábito, da resposta uma coreografia e do postmortem a principal alavanca de melhoria.

Com isso, a crise deixa de ser cataclismo e vira uma rotina acionada pelo evento: um procedimento trivial, treinado e coordenado — não um acontecimento transcendental.

## Princípios orientadores

Confiabilidade como feature: Disponibilidade, latência, qualidade e segurança são objetivos explícitos medidos por SLOs; incidentes são violações reais ou potenciais desses objetivos.

Calma operacional: As pessoas performam como treinam; padronize rituais de resposta para reduzir carga cognitiva em alta pressão.

Segurança psicológica e cultura sem culpa: trate incidentes como oportunidades de aprendizado; investigue condições sistêmicas, não indivíduos.

Comunicação clara e frequente: informação oportuna reduz ruído, alinha decisões e mantém a confiança de clientes e liderança.

Automação e repetibilidade: tudo que é crítico e repetitivo deve ser automatizado (detecção, contenção, rollback, coleta de evidências).

## Preparação (antes do incidente)

Defina SLOs e Error Budgets: por serviço, com sinais de usuário. Use o orçamento de erro para governança de risco (gate de lançamentos, hardening).

Catálogo de serviços e ownership: Cada serviço com on-call rotacionado, contatos, runbooks, dashboards, dependências e níveis de criticidade.

Playbooks e runbooks: procedimentos para classes comuns de falhas (ex.: saturação, regressão de deploy, incidentes de segurança, perda de região).

Treinamento e exercícios: GameDays e simulações (incl. “Diabo na caixa”/chaos engineering) que testam monitoramento, failover e comunicação.

Ferramentas: Monitoramento, tracing, logs centralizados, feature flags, deploy canário/blue-green, circuit breakers, gestão de incidentes (war room, timelines, status page), e chatops.

Matriz de severidade e papéis: critérios objetivos para SEV0/SEV1/SEV2; função de Incident Commander (IC), Communications Lead, Operations/Tech Lead, Scribe, Liaison com clientes.

## Detecção e triagem

Detecção proativa: alertas confiáveis com sinais acionáveis (SLO/SLA, saturação de recursos, filas, erros 5xx, p95/p99). Evite *alert fatigue* (excesso de alerta faz com que todos sejam ignorados) com limites e deduplicação.

Qualificação rápida: Em 5–10 minutos, classifique severidade e escopo. Se não há certeza, escale. Prefira over-triage à subestimativa.

Congelamento de mudanças: Em SEV alto, pause deploys e alterações não relacionadas até estabilização.

## Comando, controle e comunicação

Estabeleça o IC: uma pessoa responsável pelo processo, não pelo debug técnico. Define prioridades, controla quem fala e quando.

Canal único de verdade: Canal de incidente dedicado (chat), com bot para carimbar eventos, mais um documento/timeline vivo.

Comunicação externa: Atualizações regulares em cadência definida (ex.: a cada 15–30 min) para stakeholders e página de status. Sem especulação; foque em fatos, impacto, ações e ETA quando confiável.

Comunicação interna: curta e operativa. Use padrões: “Situação, Impacto, Ações, Próximos passos” (SIAP).

## Diagnóstico e estabilização

Objetivo primário: Restaurar serviço de forma segura o mais rápido possível (MTTR baixo), mesmo com mitigação temporária.

Estratégia: Trabalhe hipóteses de maior probabilidade/impacto primeiro; use árvores de falhas, change graphs e “o que mudou?” (deploys, feature flags, tráfego).

Técnicas de mitigação: Rollback/canário, isolar componentes, rate limiting, feature kill switches, ampliar capacidade, failover regional, aquecimento de caches, circuit breaking.

Preservação de evidências: Salve métricas, logs, perfis e snapshots para análise posterior.

## Resolução e recuperação

Critérios de “estável”: SLOs normalizados por um período definido; filas drenadas; erro residual entendido.

Encerramento progressivo: abaixe a severidade quando estabilizado; remova mitigação temporária com cautela; retome mudanças com change windows.

Documente o que foi feito: Timeline com ações, decisões e resultados. Isso alimenta o postmortem.

## Pós-incidente e aprendizado

Postmortem sem culpa: Publicado em até X dias, com descrição do impacto (usuário, duração, SLO/SLA), linha do tempo, causas (proximais e sistêmicas), o que funcionou/não funcionou, e ações corretivas.

Ações corretivas SMART: Donos, prazos, métricas de sucesso. Inclua hardening de alertas, testes, automações de rollback, melhorias em runbooks e treinamento.

Revisão de portfólio: Agregue tendências (frequência, classes de falha, “mean time to acknowledge/resolve”), identifique riscos sistêmicos e priorize engenharia de confiabilidade.

Transparência com clientes: Para SEVs críticos, publique um resumo do postmortem, próximos passos e prazos.

## Integração com governança e negócios

Error budget como freio de mudança: Ao estourar o orçamento, foque em confiabilidade (bugfix, toil, débitos de observabilidade) e aplique change freezes.

Alinhamento com risco e compliance: Integre gestão de incidentes com requisitos de segurança, privacidade e continuidade (ex.: RTO/RPO).

Métricas executivas: Reporte disponibilidade, SLOs, incidentes por severidade, tempo de detecção/confirmação/resolução, custo de indisponibilidade e tendência.

# Considerações específicas para SREs

SRE como facilitador: Lidera o processo de incidente, mas delega debug para donos dos serviços. Foco em sistema socio-técnico.

Redução de toil durante crises: Use automação para criação de salas, templates, coleta de dados e publicações em status page.

Design para falhas: Cotas, timeouts, retries com backoff, idempotência, limites de concorrência, bulkheads, isolamento por tenant e degradação graciosa.

Observabilidade orientada a usuário: Dashboards de “golden signals” (latência, tráfego, erros, saturação) e rastreamento de jornadas críticas.

## Checklists operacionais

D-0 (pré-incidente): SLOs e páginas de serviço atualizados; runbooks testados; on-call com cobertura e backups; exercícios trimestrais.

D+0 (primeiros 15 min): Nomeie IC, defina severidade, congele mudanças, comunique o primeiro update, investigue “o que mudou”.

D+0 a resolução: Mitigação primeiro, causa depois; mantenha atualizações; documente timeline; capture evidências.

D+1 a D+7: Postmortem, ações corretivas criadas e priorizadas; acompanhamento executivo se SEV alto.

D+30: Verificação de eficácia das ações; ajuste de SLOs/alertas se necessário.

### Exemplo de fluxos e artefatos

* Template de atualização externa:
    

“Estamos investigando \[sintoma\]. Impacto: \[quem e como\]. Próximo update às \[hora\]. Mitigações em andamento: \[ações\].”

* Template de postmortem:
    

Resumo executivo; Impacto; Linha do tempo; Detecção; Resposta; Causas; Lições; Ações corretivas; Anexos (gráficos, logs).

* Política de severidade (exemplo):
    

SEV0: indisponibilidade total de função crítica com impacto generalizado.

SEV1: degradação ou indisponibilidade parcial de função crítica.

SEV2: impacto moderado com workaround.

SEV3: impacto baixo, sem impacto percebido amplo.

Também conhecido como P0, P1, P2, P3…