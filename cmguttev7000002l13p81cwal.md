---
title: "SRE e as boas práticas para gestão de incidentes"
datePublished: Fri Oct 17 2025 12:29:53 GMT+0000 (Coordinated Universal Time)
cuid: cmguttev7000002l13p81cwal
slug: sre-e-as-boas-praticas-para-gestao-de-incidentes
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/1cmUs5eXOtg/upload/65a338fbec3c967b18260d05975124c6.jpeg
tags: checklist, sre, incident-management, incident-response, crisis, crisis-management, crises

---

Incidentes e crises: Se não há como prevê-las, há como estudarmos o que já aconteceu para a criação de planos mais adequados.

Um complemento do artigo anterior em [https://esli.blog.br/sre-e-o-gerenciamento-de-crises-e-incidentes](https://esli.blog.br/sre-e-o-gerenciamento-de-crises-e-incidentes)

%[https://esli.blog.br/sre-e-o-gerenciamento-de-crises-e-incidentes] 

As seis fases da resposta a incidentes são basicamente:

**preparação** – o objetivo é deixar usuários e profissionais de TI prontos para lidar com os potenciais incidentes no caso de acontecerem, e, como sabemos, eles ocorrerão independentemente da nossa vontade;

**identificação** – o objetivo é descobrir o que é o “incidente de segurança”, para ter clareza de quais eventos se pode ignorar e quais eventos exigem uma ação imediata;

**contenção** – o objetivo passa a ser o isolamento dos sistemas afetados para prevenir danos adicionais, e, normalmente, quarentenas automatizadas são as ações mais indicadas;

**erradicação** – o objetivo é encontrar e eliminar a causa raiz, mais comumente chamada pela expressão em inglês Root Cause Analysis (RCA), removendo sistemas de produção afetados;

**recuperação** – o objetivo é permitir que os sistemas afetados voltem ao ambiente de produção e a situação se normalize definitivamente

**lições aprendidas** – o objetivo é ter todo o incidente documentado e fazer uma análise com todos os membros da equipe sobre o que se pode fazer para ter a melhor resposta para incidentes futuros.

## Boas Práticas de SRE para Gestão de Incidentes

1. **Definição Clara de Incidente**:
    
    * Definir o que constitui um incidente para a equipe, garantindo que todos tenham a mesma compreensão.
        
2. **Monitoramento Proativo**:
    
    * Implementar monitoramento eficaz para detectar incidentes o mais rápido possível.
        
    * Utilizar métricas e logs para identificar anomalias.
        
3. **Alertas Eficazes**:
    
    * Configurar alertas que sejam acionáveis e relevantes, evitando a fadiga de alertas falsos.
        
    * Priorizar alertas críticos que afetam a disponibilidade e a performance.
        
4. **Equipe de Resposta a Incidentes**:
    
    * Ter uma equipe dedicada a responder a incidentes, com papéis e responsabilidades bem definidos.
        
    * Realizar treinos regulares e simulações de incidentes para preparar a equipe.
        
5. **Comunicação Clara**:
    
    * Estabelecer canais de comunicação eficazes durante incidentes, tanto internamente quanto externamente com os stakeholders.
        
    * Manter a transparência sobre o andamento da resolução do incidente.
        
6. **Documentação e Registro**:
    
    * Documentar todos os incidentes e as ações tomadas durante a resposta.
        
    * Manter registros para análise futura e aprendizado.
        
7. **Análise Pós-Mortem**:
    
    * Realizar análises pós-mortem para identificar causas raízes e áreas de melhoria.
        
    * Compartilhar aprendizados e implementar mudanças para evitar recorrências.
        

## Checklist de Tratamento de Incidentes

#### 1\. Recebimento do Alerta Inicial

* Verificar a origem do alerta (monitoramento, usuário, etc.).
    
* Avaliar a gravidade e o impacto do incidente.
    
* Notificar a equipe de resposta a incidentes.
    

#### 2\. Avaliação Inicial

* Reunir a equipe de resposta a incidentes.
    
* Designar um líder de incidentes (Incident Commander).
    
* Reunir informações preliminares (logs, métricas, etc.).
    

#### 3\. Investigação e Diagnóstico

* Identificar a natureza e a extensão do incidente.
    
* Analisar dados e logs para identificar padrões ou causas.
    
* Realizar reuniões rápidas (stand-ups) para atualizar o status.
    

#### 4\. Resolução do Incidente

* Desenvolver um plano de ação para resolver o incidente.
    
* Implementar a solução ou workaround.
    
* Monitorar a eficácia da solução aplicada.
    

#### 5\. Comunicação Durante o Incidente

* Atualizar stakeholders sobre o status do incidente.
    
* Informar usuários finais sobre a situação e o progresso da resolução.
    

#### 6\. Encerramento do Incidente

* Confirmar que o incidente foi resolvido e que os serviços estão funcionando corretamente.
    
* Documentar as ações tomadas e as soluções implementadas.
    

#### 7\. Análise Pós-Mortem

* Realizar uma reunião de análise pós-mortem com a equipe.
    
* Identificar causas raízes e áreas para melhoria.
    
* Documentar os aprendizados e as recomendações.
    
* Compartilhar os resultados da análise com toda a organização.
    

#### 8\. Implementação de Melhorias

* Implementar ações corretivas para evitar recorrências.
    
* Atualizar documentação e processos com base nos aprendizados.
    
* Revisar e melhorar o plano de resposta a incidentes.