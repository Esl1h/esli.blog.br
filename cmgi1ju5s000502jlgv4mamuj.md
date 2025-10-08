---
title: "SRE e Métricas"
datePublished: Wed Oct 08 2025 13:45:23 GMT+0000 (Coordinated Universal Time)
cuid: cmgi1ju5s000502jlgv4mamuj
slug: sre-e-metricas
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/-yBvef_mAaQ/upload/2eb248d79da64b059ec98ece5d0f54e4.jpeg
tags: metrics, devops, sre, observability, sre-devops, monitoramento, dora, mean-time

---

Compreender e implementar um conjunto robusto de métricas não é somente uma boa prática, mas uma necessidade estratégica, uma obrigação.

Uma visão das métricas utilizadas, incluindo as quatro métricas clássicas do framework DORA (DevOps Research and Assessment) - que recentemente expandiu para cinco métricas com a adição da taxa de retrabalho -, os quatro Golden Signals do Google para monitoramento de sistemas distribuídos, e a família completa dos Mean Times, formando a base da confiabilidade.

Em um contexto onde a transformação digital acelera a complexidade dos sistemas e a adoção de IA em desenvolvimento está remodelando os fluxos de trabalho, essas métricas servem como bússola para navegar entre a necessidade de inovação rápida e a manutenção da estabilidade operacional.

Num papo recente, fui categórico: o SRE é o guardião das métricas, ele, mais que os outros, precisa ter claramente toda a visão de negócio, visão de produto… ter o monitoramento, controlar os painéis e métricas de confiabilidade. Decisões de código, produto e planejamentos futuros na empresa irão usar dados do SRE como baliza, precisamos passar total confiança e ter conhecimento amplo para exercer nossa atividade e entregar estes nossos resultados.

Não tem como ser a referência de confiabilidade sem ter o total domínio da linguagem de negócio e produtos, e isto fará com que o SRE crie métricas que façam sentido para toda a empresa, mas antes deste ponto, precisa ter estas abaixo com total clareza.

Reforço o foco na clareza das informações (documentação objetiva), então, se chegou aqui e não tem uma documentação concisa, volte. Aqui falei um pouco mais sobre as monitorações: [https://esli.blog.br/as-monitoracoes-do-sre](https://esli.blog.br/as-monitoracoes-do-sre) e também sobre as documentações do SRE: [https://esli.blog.br/as-documentacoes-do-sre](https://esli.blog.br/as-documentacoes-do-sre)

Lembre-se, na indústria, para confiabilidade e manutenabilidade/mantenabilidade, existe até a norma ABNT NBR 5462/1994 (para indústria e equipamentos), logo, SREs, DevOps e afins precisam no mínimo ter uma documentação de respeito e métricas coletadas que não deixam brecha alguma para serem questionadas - não passe vergonha ;-)

# DORA, 4 Golden Signals e os Mean Times

1. **As 4 métricas DORA (DevOps Research and Assessment):**
    
    * Frequência de Implantação (Deployment Frequency)
        
    * Tempo de Lead para Mudanças (Lead Time for Changes)
        
    * Tempo Médio de Recuperação (Mean Time to Recovery - MTTR)
        
    * Taxa de Mudança de Falhas (Change Failure Rate)
        
2. **Os 4 Golden Signals:**
    
    * Latência (Latency)
        
    * Tráfego (Traffic)
        
    * Erros (Errors)
        
    * Saturação (Saturation)
        

## DORA:

1. **Frequência de Implantação (Deployment Frequency):** Com que frequência uma organização lança com sucesso novos recursos ou código para produção.  
    
2. **Tempo de Lead para Mudanças (Lead Time for Changes):** Quanto tempo leva para uma alteração no código ser implantada em produção.  
    
3. **Tempo Médio de Recuperação (Mean Time to Recovery - MTTR):** Quanto tempo leva para uma organização se recuperar de uma falha em produção.  
    
4. **Taxa de Mudança de Falhas (Change Failure Rate):** A porcentagem de mudanças em produção resultantes em incidentes ou falhas.
    

## Quatro Golden signals:

1. **Latência (Latency):** O tempo que leva para atender a uma solicitação. É importante monitorar tanto a latência média quanto a latência em percentis mais altos (como P95, P99), pois valores altos podem indicar problemas para uma parcela significativa dos usuários.  
    
2. **Tráfego (Traffic):** Uma medida de quanta demanda está sendo colocada no seu sistema. Isso pode ser medido em solicitações por segundo, largura de banda da rede ou qualquer outra métrica relevante para o seu sistema.  
    
3. **Erros (Errors):** A taxa de solicitações que falham. É importante monitorar tanto erros explícitos (como códigos de erro HTTP) quanto erros implícitos (como respostas incorretas ou dados corrompidos).  
    
4. **Saturação (Saturation):** Mede o quão "cheio" seus recursos estão. Isso pode se referir ao uso da CPU, memória, disco ou rede. A alta saturação pode indicar que o sistema está próximo do seu limite de capacidade e pode começar a ter problemas de desempenho.
    

## MTs:

* **MTBF (Mean Time Between Failures):** Tempo médio entre falhas. Mede a confiabilidade de um sistema ou componente, indicando o tempo médio que ele opera sem falhas. É geralmente usado para sistemas reparáveis. Um MTBF mais alto indica maior confiabilidade.  
    
* **MTTR (Mean Time To Repair):** Tempo médio para reparar. Mede a mantenabilidade de um sistema, indicando o tempo médio necessário para diagnosticar e reparar uma falha. Um MTTR mais baixo indica maior facilidade de manutenção.  
    
* **MTTF (Mean Time To Failure):** Tempo médio até a falha. Semelhante ao MTBF, mas usado para sistemas não reparáveis. Indica o tempo médio que um sistema ou componente opera antes de falhar completamente e precisar ser substituído.  
    
* **MTTA (Mean Time To Acknowledge):** Tempo médio para reconhecer. Usado em contextos de monitoramento e resposta a incidentes, mede o tempo médio que leva para um membro da equipe reconhecer um alerta ou incidente após sua ocorrência.  
    
* **MTTI (Mean Time To Identify):** Tempo médio para identificar. Semelhante ao MTTA, mas se refere ao tempo médio necessário para identificar a causa raiz de um problema ou incidente.  
    
* **MTTV (Mean Time To Validate):** Tempo médio para validar. Tempo médio necessário para validar que uma correção ou solução resolveu o problema original.  
    
* **MTTD (Mean Time To Detect):** Tempo médio para detectar. Mede o tempo médio que leva para detectar uma falha ou incidente no sistema. É crucial para minimizar o impacto de problemas, permitindo uma resposta mais rápida.  
    
* **MTTS (Mean Time To Switchover):** Tempo médio para comutação. Em sistemas redundantes ou de alta disponibilidade, mede o tempo médio necessário para realizar uma comutação (switchover) para um sistema de backup em caso de falha.
    

%[https://esli.blog.br/as-monitoracoes-do-sre] 

%[https://esli.blog.br/as-documentacoes-do-sre]