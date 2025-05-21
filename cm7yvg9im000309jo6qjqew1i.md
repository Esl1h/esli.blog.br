---
title: "As monitorações do SRE"
seoTitle: "O que é monitoramento SRE?"
seoDescription: "Explore 8 diretrizes essenciais de monitoramento SRE, desde SLIs até segurança integrada, para otimizar a confiabilidade e automação do sistema"
datePublished: Fri Mar 07 2025 14:29:23 GMT+0000 (Coordinated Universal Time)
cuid: cm7yvg9im000309jo6qjqew1i
slug: as-monitoracoes-do-sre
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/JKUTrJ4vK00/upload/206b2d8ea6f1f668ca3b627af57c4f84.jpeg
tags: monitoring, sre, observability, sre-devops, monitoramento

---

No artigo anterior, explorei as documentações do SRE:

%[https://esli.blog.br/as-documentacoes-do-sre] 

Onde são:

* PRR
    
* PostMortem
    
* Service overview
    
* Playbooks
    
* Políticas
    

Vamos explorar agora as monitorações do SRE.

Separei em 8 diretrizes a serem seguidas e na qual o monitoramento deve ser estruturado.

## **1\. Definição de SLIs, SLOs e SLAs**

* **Descrição:** O monitoramento deve medir e rastrear os Indicadores de Nível de Serviço (SLIs), como latência, taxa de erros, disponibilidade e saturação. Estes são usados para avaliar se os Objetivos de Nível de Serviço (SLOs) estão sendo atendidos.
    
* **Como fazer:**
    
* * Identifique os SLIs mais relevantes para o seu sistema (por exemplo, tempo de resposta da API, taxa de sucesso das transações, uso de memória/CPU).
        
    * Defina metas claras para os SLOs. Por exemplo: "95% das requisições devem ter latência abaixo de 200ms".
        
    * Monitore em tempo real para verificar se os SLAs (contratos com clientes) estão sendo atendidos.
        
* **Evidências:** Painéis de monitoramento que exibam métricas relacionadas a SLIs/SLOs.
    

---

## **2\. Monitoramento baseado em sintomas, não em causas**

* **Descrição:** O monitoramento deve detectar sintomas de problemas para notificar a equipe antes que os usuários sejam impactados, em vez de tentar prever todas as causas possíveis de falhas.
    
* **Como fazer:**
    
* * Configure alertas com base em SLIs (por exemplo, aumento na taxa de erros ou na latência).
        
    * Evite alertas baseados em métricas de infraestrutura isoladas (como CPU alta), a menos que estejam correlacionadas com impacto no usuário.
        
* **Evidências:** Relatórios de eventos que mostrem alertas acionados com base em sintomas.
    

### **Monitoramento dos Quatro Sinais de Ouro (4 Golden Signals):**

* **Latência:** Meça o tempo de resposta das solicitações, diferenciando entre solicitações bem-sucedidas e com falha, para avaliar a experiência do usuário.
    
* **Tráfego:** Acompanhe a quantidade de demanda no sistema, como o número de solicitações por segundo, para entender o uso e identificar possíveis sobrecargas.
    
* **Erros:** Monitore a taxa de solicitações que resultam em erros, incluindo códigos de status HTTP 500 e outros tipos de falhas, para manter a integridade do sistema.
    
* **Saturação:** Avalie a capacidade do sistema, monitorando o uso de recursos como CPU e largura de banda, para prevenir degradações de desempenho.
    

Configure alertas que sejam acionados quando os SLOs estiverem em risco de serem violados, garantindo que a equipe seja notificada apenas em situações que exigem intervenção.

---

## **3\. Automação e Resposta a Incidentes**

* **Descrição:** Minimize a necessidade de intervenção manual por meio de automação e defina processos claros para gerenciar incidentes.
    
* **Como fazer:**
    
* * Configure automações para resolver problemas comuns, como reiniciar serviços ou escalar recursos automaticamente.
        
    * Use um sistema de gerenciamento de incidentes para coordenar respostas rápidas.
        
    * Monitore o tempo de resolução de incidentes e revise os processos regularmente.
        
* **Evidências:** Logs de automação, playbooks de resposta a incidentes e relatórios pós-morte.
    

Um alerta pode disparar um trigger para um webhook que por sua vez, executa algum script que resolverá automaticamente o problema alarmado.

---

## **4\. Redução do Toil**

* **Descrição:** Reduza tarefas manuais repetitivas e operacionais (toil) por meio de automação. O monitoramento deve suportar essa abordagem.
    
* **Como fazer:**
    
* * Automatize a coleta de métricas e a geração de relatórios.
        
    * Implemente ferramentas de observabilidade que correlacionem logs, métricas e traços em um só lugar.
        
* **Evidências:** Implementação de ferramentas como Prometheus, Grafana, Elastic Stack ou Datadog para centralizar e automatizar o monitoramento.
    

---

## **5\. Observabilidade e Diagnóstico**

* **Descrição:** O sistema deve ser observável, permitindo que a equipe investigue e diagnostique problemas rapidamente.
    
* **Como fazer:**
    
* * Use logs estruturados, métricas e rastreamento distribuído (distributed tracing) para obter visibilidade de ponta a ponta.
        
    * Adote ferramentas como OpenTelemetry ou Jaeger para rastreamento distribuído.
        
    * Configure dashboards para correlacionar métricas de diferentes componentes do sistema.
        
* **Evidências:** Relatórios de incidentes que mostrem como as ferramentas de observabilidade ajudaram no diagnóstico.
    

---

## **6\. Gestão de Capacidade e Saturação**

* **Descrição:** Monitore a capacidade do sistema para evitar saturação e gerencie limites de recursos.
    
* **Como fazer:**
    
* * Configure alertas para níveis críticos de utilização de CPU, memória, largura de banda ou outros recursos.
        
    * Realize testes de carga regularmente para prever gargalos.
        
* **Evidências:** Relatórios de testes de carga e dashboards que mostrem métricas de capacidade.
    

---

## **7\. Blameless Postmortems (Análise sem Culpados)**

* **Descrição:** Após incidentes, conduza análises pós-morte para documentar causas, impactos e ações corretivas, sem atribuir culpa.
    
* **Como fazer:**
    
* * Estruture as análises pós-morte com perguntas como: "O que aconteceu?", "Por que aconteceu?" e "Como evitar que aconteça novamente?".
        
    * Atualize processos de monitoramento e automação com base nos aprendizados.
        
* **Evidências:** Relatórios pós-morte documentados.
    

---

## **8\. Práticas de Segurança e Confiabilidade**

* **Descrição:** Incorpore práticas de segurança ao monitoramento para detectar atividades maliciosas e garantir a confiabilidade.
    
* **Como fazer:**
    
* * Inclua monitoramento para detectar anomalias de segurança, como tentativas de login inválidas ou tráfego suspeito.
        
    * Garanta que todos os dados de monitoramento sejam protegidos contra acessos não autorizados.
        
* **Evidências:** Logs de segurança e auditoria.
    

---

## Exemplo e Aplicação

### **Como deve ser o sistema de monitoramento do seu sistema**

Com base nos princípios acima, o sistema de monitoramento deve atender aos seguintes critérios:

1. **Centralização de Métricas e Logs:**
    
    * Utilize ferramentas como **Prometheus**, **Grafana**, **Datadog** ou **Elastic Stack** para coletar e exibir métricas em tempo real.
        
    * Implemente logs estruturados com ferramentas como **Fluentd** ou **Logstash**.
        
2. **Alertas Inteligentes:**
    
    * Configure alertas baseados em SLIs diretamente correlacionados ao impacto no usuário.
        
    * Utilize ferramentas como **Alertmanager**, **PagerDuty, Cabot, Healthchecks** para gerenciar alertas.
        
3. **Observabilidade Completa:**
    
    * Adote práticas de rastreamento distribuído com ferramentas como **Jaeger**, **OpenTelemetry, Haystack, Skywalking** ou **Zipkin**.
        
    * Monitore métricas de infraestrutura, aplicação e experiência do usuário final.
        
4. **Automação e Resiliência:**
    
    * Automatize respostas a incidentes sempre que possível.
        
    * Implemente backups e failovers automáticos para aumentar a resiliência.
        
5. **Relatórios e Dashboards:**
    
    * Crie dashboards claros e acionáveis para exibir saúde do sistema, SLIs/SLOs e alertas.
        
    * Automatize relatórios semanais/mensais de métricas e incidentes.
        
6. **Testes de Capacidade:**
    
    * Realize testes regulares de carga e estresse para identificar gargalos antes que eles impactem os usuários.
        
7. **Segurança Integrada:**
    
    * Monitore logs de segurança e configure alertas para atividades suspeitas.
        
    * Proteja o sistema de monitoramento contra acessos não autorizados.