---
title: "Paper: dar mais tempo de “raciocínio” para AI piora o desempenho."
seoTitle: "Raciocínio Prolongado Reduz Desempenho da IA"
seoDescription: "Dar mais tempo de raciocínio para IA pode piorar o desempenho, revela estudo sobre escalonamento inverso em computação"
datePublished: Thu Jul 24 2025 22:17:50 GMT+0000 (Coordinated Universal Time)
cuid: cmdhye3qj000d02i14sa77l0d
slug: paper-dar-mais-tempo-de-raciocinio-para-ai-piora-o-desempenho
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/OAsF0QMRWlA/upload/3a82cc0946aea3442a86b19609a52863.jpeg
tags: ai, ia, openai, llm, claude, llms

---

Aumentar a capacidade de “reasoning” para os Large Reasoning Models (LRMs) piora o desempenho e a precisão.

A indústria tem investido pesado em “test-time compute” (dar mais tempo de processamento para o modelo), esperando ganhos de desempenho. O estudo evidencia que essa estratégia pode reforçar padrões de raciocínio problemáticos.

Pesquisadores da Anthropic evidenciaram: dar mais tempo de “raciocínio” para o modelo nem sempre melhora o desempenho — e, em muitos casos, pode até piorar. Esse efeito foi chamado de “inverse scaling in test-time compute” (teoria do “escalonamento inverso no tempo de computação”). O estudo demonstra que, ao aumentar o tempo de processamento para tarefas de raciocínio, a precisão dos modelos pode cair, desafiando a crença comum de que mais computação sempre resulta em melhores respostas.

Foram testadas 4 categorias de tarefas nos modelos de AI: problemas simples de contagem com distrações, tarefas de regressão com recursos enganosos, quebra-cabeças complexos de dedução e cenários envolvendo preocupações de segurança de IA.

E com isto observou o “Escalonamento Inverso”: Em vez de melhorar, o desempenho dos modelos pode piorar quando recebem mais tempo para pensar em tarefas específicas. Revelando padrões de falhas nos principais sistemas de AI. Como:

\- Modelos Claude tendem a se distrair com informações irrelevantes quando raciocinam por mais tempo.

\- Modelos GPT (OpenAI) resistem a distrações, mas acabam “overfitting” (ajustando demais) ao formato do problema.

\- Os modelos deixam de usar suposições lógicas e começam a encontrar relações que não fazem sentido.

\- Os modelos apresentam dificuldades em manter o foco em tarefas dedutivas complexas.

\- o raciocínio prolongado pode amplificar comportamentos preocupantes.

Ou seja, quanto mais token gasta, pior fica a precisão.

Resultados: A relação entre investimento computacional e desempenho de IA é mais complexa do que se pensava e o “overthinking” pode ser um dos maiores inimigos da IA e não a falta de poder computacional.

![Imagem](https://pbs.twimg.com/media/Gwc8g2HbEAMiyy1?format=jpg&name=4096x4096 align="left")

O paper completo: [https://arxiv.org/pdf/2507.14417](https://arxiv.org/pdf/2507.14417)

Código, demo e todo o material que demonstra a tese está no github: [https://safety-research.github.io/inverse-scaling-ttc/](https://safety-research.github.io/inverse-scaling-ttc/)

Thread no X de pesquisador mostrando os resultados: [https://x.com/aryopg/status/1947591901886222570](https://x.com/aryopg/status/1947591901886222570)