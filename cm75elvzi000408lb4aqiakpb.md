---
title: "Inteligência Artificial (parte 2): Entenda AI Generativa, Redes Neurais, LLMs e Tokens"
seoTitle: "AI Generativa e Modelos de Linguagem"
seoDescription: "Explore a diferença entre IA generativa e grandes modelos de linguagem, seus usos, desafios e impacto na criação de conteúdo e comunicação"
datePublished: Fri Feb 14 2025 23:32:33 GMT+0000 (Coordinated Universal Time)
cuid: cm75elvzi000408lb4aqiakpb
slug: inteligencia-artificial-parte-2-entenda-ai-generativa-redes-neurais-llms-e-tokens
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ZPOoDQc8yMw/upload/fcb7b7e9448902a3c19f5218975be363.jpeg
tags: ai, opensource, ia, chatbot, ai-tools, llm, chatgpt, aitools, llms

---

Complementando o primeiro artigo em:

%[https://esli.blog.br/inteligencia-artificial-evolucao-principais-empresas-e-seu-futuro] 

IA (AI) Generativa e Grandes Modelos de Linguagem (LLMs, Large Language Models - ainda não me acostumei com alguma tradução para estes termos - e AI/IA confundem rsrss) são frequentemente usados de forma intercambiável, mas embora compartilhem algumas semelhanças, eles diferem significativamente em propósito, arquitetura e recursos.

## O que é IA generativa?

AI generativa refere-se a uma classe de sistemas de IA projetados para criar novos conteúdos, seja texto, imagens, música ou até mesmo vídeo, com base em padrões aprendidos a partir de dados existentes.

### Como funciona a IA Generativa?

Em sua essência, a IA generativa funciona aprendendo padrões a partir de abundantes quantidades de dados, como imagens, texto ou sons.

O processo envolve alimentar os enormes conjuntos de dados da IA, permitindo que ela “entenda” esses padrões profundamente o suficiente para recriar algo semelhante, mas totalmente original.

O aspecto "gerativo" significa que a IA não apenas reconhece ou classifica informações; ela produz algo novo do zero.

### 1\. Redes Neurais

Usos generativos de IA redes neurais, que são algoritmos inspirados em como o cérebro humano funciona.

Essas redes consistem em camadas de neurônios artificiais, cada um responsável pelo processamento de dados.

As redes neurais podem ser treinadas para reconhecer padrões nos dados e, em seguida, gerar novos dados que sigam esses padrões.

### 2\. Redes Neurais Recorrentes (RNNs)

Para tarefas que envolvem sequências, como gerar texto ou música Redes Neurais Recorrentes (RNNs) são frequentemente utilizados.

RNNs são um tipo de rede neural projetada para processar dados sequenciais, mantendo uma espécie de "memória" do que veio antes.

Por exemplo, ao gerar uma frase, RNNs lembrar as palavras que foram previamente gerados, permitindo-lhes criar frases coerentes em vez de cadeias aleatórias de palavras.

### 3\. Redes Adversariais Generativas (GANs)

GANs trabalham colocando duas redes neurais umas contra as outras. Gerador Vs Discriminador

Uma rede, o Gerador, cria conteúdo (como uma imagem), enquanto a outra rede, o Discriminador julga se esse conteúdo parece real ou falso.

O Gerador aprende com o feedback do Discriminador, melhorando gradualmente até que possa produzir conteúdo que seja indistinguível de dados reais.

Este método é particularmente eficaz na geração de imagens e vídeos de alta qualidade.

## Exemplos de IA Generativa

### Geradores de imagem:

**DALL-E:** Pode gerar imagens altamente detalhadas a partir de descrições textuais, demonstrando sua capacidade de entender e traduzir a linguagem em forma visual.

**Stable Diffusion:** Permite aos utilizadores gerar uma vasta gama de imagens, desde retratos realistas a paisagens fantásticas.

E já falei mal de como estão sendo treinados, lá em 2022, pois estão gerando com base em artistas (na verdade, copiando muito descaradamente, naquela época):

%[https://esli.blog.br/lensa-ai-art-stable-diffusion-nao-obrigado] 

### Geradores de música:

**Udio**: Esta ferramenta AI pode criar composições musicais originais em vários estilos, do clássico ao eletrônico.

**Jukebox**: Outro gerador de música notável, Jukebox é capaz de gerar música realista em diferentes gêneros e até mesmo imitar artistas específicos.

### Ferramentas de vídeo:

**Pista**: Esta plataforma versátil oferece um conjunto de ferramentas para edição de vídeo, animação e geração. Ele pode ser usado para criar tudo, desde animações simples até efeitos visuais complexos.

**Topaz**: Este software é especializado em melhorar e restaurar imagens de vídeo, usando IA para melhorar a qualidade, reduzir o ruído e até aumentar a resolução.

## O que são os Grandes Modelos de linguagem (LLMs)?

Grandes Modelos de Linguagem (LLMs) são uma forma especializada de inteligência artificial projetada para entender e gerar linguagem humana com notável proficiência.

Ao contrário da IA generativa geral, que pode criar uma variedade de conteúdo, os LLMs se concentram especificamente no processamento e produção de texto, tornando-os parte integrante de tarefas como tradução, sumarização e IA de conversação.

### Como os LLMs Funcionam

Em seu núcleo, os LLMs alavancam Processamento de Linguagem Natural (PNL) um ramo da IA dedicado à compreensão e interpretação da linguagem humana. O processo começa com a tokenização.

## Token/Tokenização

A divisão de uma frase em unidades menores, geralmente palavras ou subpalavras.

Elas são chamadas de tokens em termos de LLM.

Esses tokens servem como blocos de construção para a compreensão do modelo.

O "Token" é muito falado (é como as LLMs/AI estão cobrando em muitas partes, seus usuários).  
É uma daquelas palavras-chave que são jogadas no ar, mas poucas pessoas param para explicá-lo de uma maneira que seja realmente compreensível.

A token em Modelos de Linguagem Grande é basicamente um pedaço de texto que o modelo lê e entende.

Pode ser tão curto quanto uma única letra ou tão longo quanto uma palavra ou mesmo parte de uma palavra. Pense nisso como a unidade de linguagem que um modelo de IA usa para processar informações.

Em vez de ler frases inteiras de uma só vez, ele as divide nessas pequenas peças digeríveis - fichas.

Uma vez que um texto é tokenizado, um modelo de linguagem pode analisar cada token para entender seu significado e contexto. Isso permite que o modelo:

* **Entenda o significado:** O modelo pode reconhecer padrões e relações entre tokens, ajudando-o a entender o significado geral de um texto.
    
* **Gerar texto:** Ao analisar os tokens e seus relacionamentos, o modelo pode gerar um novo texto, como completar uma frase, escrever um parágrafo ou até mesmo compor um artigo inteiro.
    

Quando falamos de tokenização no contexto de Grandes Modelos de Linguagem (LLMs), é importante entender que existem diferentes métodos para dividir o texto em tokens:  
**1\. Tokenização em Nível de Palavra**

**2\. Tokenização de Sub-Nível de Sub-Senha (ex.: prefixos, sufixos)**

**3\. Tokenização em Nível de Caracteres**

**4\. Tokenização em Nível de Bytes (textos multilíngues, outros alfabetos, ...)**

### Limite de Token

Um limite refere-se ao número máximo de tokens que um LLM pode processar em uma única entrada, incluindo o texto de entrada e a saída gerada.

Pense como o volume de dados que o modelo pode armazenar e processar de uma só vez. Quando você exceder esse limite, o modelo interromperá o processamento ou truncará a entrada.

Por exemplo, o GPT-3 pode lidar com até 4096 tokens, enquanto o GPT-4 pode processar até 8192 ou até 32.768 tokens, dependendo da versão.

Isso significa que tudo na interação, desde o prompt que você envia até a resposta, deve se encaixar nesse limite.

### Por Que os Limites de Token Importam?

* **Compreensão Contextual**: Os LLMs dependem de tokens anteriores para gerar respostas contextualmente precisas e coerentes.
    
* * Se o modelo atingir seu limite de token, ele perderá o contexto além desse ponto, o que pode resultar em saídas menos coerentes ou incompletas.
        
* **Truncamento de Entrada**: Se sua entrada exceder o limite de token, o modelo cortará parte da entrada, geralmente começando do início ou do fim. Isso pode levar a uma perda de informações cruciais e afetar a qualidade da resposta.
    
* **Limitação de Saída**: Se sua entrada usar a maior parte do limite de token, o modelo terá menos tokens para gerar uma resposta.
    
* * Por exemplo, se você enviar um prompt que consome 3900 tokens no GPT-3, o modelo terá apenas 196 tokens restantes para fornecer uma resposta, o que pode não ser suficiente para consultas mais complexas.
        

## Transformadores/Transformers

Os LLMs normalmente usam uma arquitetura chamada transformadores um modelo que revolucionou o processamento de linguagem natural.

Eles trabalham analisando as relações entre palavras e seus contextos em conjuntos de dados massivos.

Em termos simples, pense neles como funções auto-completas sobrecarregadas capazes de escrever ensaios, responder a perguntas complexas ou resumir artigos.

## Exemplos de LLM

**Geração de Texto:**

GPT 3: Um dos LLMs mais conhecidos. É capaz de gerar textos semelhantes aos humanos, desde escrever ensaios até criar poesia.

GPT-4: É um sucessor mais avançado e aprimorado como ter memória, o que permite manter e acessar informações de conversas anteriores.

Gemini: Um LLM notável do Google, que se concentra em melhorar a geração e a compreensão de texto.

# AI e LLMs generativos: Um vínculo único

Agora que você está familiarizado com os conceitos básicos de IA generativa e grandes modelos de linguagem (LLMs), vamos explorar o potencial transformador quando essas tecnologias são combinadas.

Aqui estão algumas ideias:

**Criação de Conteúdo**

A combinação de LLMs e IA generativa permite a criação de conteúdo único e contextualmente relevante em vários textos de mídia, imagens e até música.

**Fale com seus dados**

Empresas e indivíduos podem agora digitalizar suas informações e interagir com elas. Podendo fazer perguntas específicas sobre documentos, conteúdos, gerar resumos ou solicitar mais informações sem comprometer a privacidade.

Essa abordagem é particularmente valiosa em áreas em que a confidencialidade dos dados é crucial, como direito, saúde ou educação.

**Chatbots Aprimorados e Assistentes Virtuais**

Ninguém gosta da resposta genérica dos chatbots de atendimento ao cliente. A combinação de LLMs e IA generativa pode alimentar chatbots avançados que lidam com consultas complexas de forma mais natural.

Por exemplo, um LLM pode ajudar um assistente virtual a entender as necessidades de um cliente, enquanto a IA geradora cria respostas detalhadas e envolventes.

Projetos de código aberto como Rasa (https://rasa.com/), uma estrutura de chatbot personalizável, tornaram essa tecnologia acessível para empresas que buscam privacidade e flexibilidade.

**Tradução e Localização Melhoradas**

Quando combinados, os LLMs e a IA generativa podem melhorar significativamente a precisão da tradução e a sensibilidade cultural.

Por exemplo, um LLM poderia lidar com as nuances linguísticas de uma língua como o árabe, enquanto a IA generativa produz imagens ou conteúdo culturalmente relevantes para o mesmo público.

Projetos de código aberto como Marian NMT (https://marian-nmt.github.io/) e Unbabel Tower (https://unbabel.com/), criaram kits de ferramentas para tradução e LLM.

# Desafios e Limitações

um robô segurando uma chave que representa a IA precisa de reparo e tem desafios

Tanto a IA Generativa quanto os LLMs enfrentam desafios significativos, muitos dos quais levantam preocupações sobre suas aplicações no mundo real:

## Viés

AI e LLMs generativos aprendem com os dados nos quais são treinados. Se os dados de treinamento contiverem vieses (por exemplo, linguagem discriminatória ou estereótipos), a IA refletirá esses vieses em sua produção.

Esta questão é especialmente problemática para os LLMs, que geram texto baseado em dados de origem da Internet, muitos dos quais contêm vieses inerentes.

## Alucinações

Um problema único para LLMs é "alucinação", onde o modelo gera informações falsas ou sem sentido com confiança injustificada.

Embora a IA generativa possa criar algo visualmente incoerente que seja fácil de detectar (como uma imagem distorcida).

Mas um LLM pode sutilmente apresentar informações incorretas de uma maneira que parece totalmente plausível, tornando mais difícil de detectar.

## Intensidade de Recursos

O treinamento de IA generativa e LLMs requer vastos recursos computacionais. Não se trata apenas de poder de processamento, mas também de armazenamento e energia.

Isso levanta preocupações sobre o impacto ambiental do treinamento de IA em larga escala.

## Preocupações Éticas

A capacidade da IA generativa de produzir imitações quase perfeitas de imagens, vozes e até personalidades coloca questões éticas.

Como podemos diferenciar entre conteúdo gerado por IA e conteúdo criado pelo homem?

Com os LLMs, a questão é: como podemos evitar a disseminação de desinformação ou o uso indevido da IA para fins maliciosos?

---

Referências:

[https://itsfoss.com/llm-vs-generative-ai/](https://itsfoss.com/llm-vs-generative-ai/)

[https://itsfoss.com/llm-token/](https://itsfoss.com/llm-token/)