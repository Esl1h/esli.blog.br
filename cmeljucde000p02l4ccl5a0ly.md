---
title: "Vigilância governamental: Five Eyes, MLAT e outros"
seoTitle: "Vigilância Governamental e Acordos Globais"
seoDescription: "Saiba como acordos de vigilância internacional, como o Five Eyes e MLAT, afetam a privacidade e segurança dos dados pessoais"
datePublished: Thu Aug 21 2025 15:21:20 GMT+0000 (Coordinated Universal Time)
cuid: cmeljucde000p02l4ccl5a0ly
slug: vigilancia-governamental
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ew3-7k3sl-g/upload/d5ae9222cf7c5b7c14ea8355c5cbfba2.jpeg
tags: nsa, cryptography, backdoor, surveillance, vigilancia, sigint

---

Embora a NSA seja a mais conhecida, quase todos os países mantêm uma agência de inteligência de sinais (**SIGINT**) — como o GCHQ no Reino Unido, DGSE na França e o BND na Alemanha. Essas agências interceptam comunicações por diversos meios (telefones, e-mails, XKEYSCORE, etc…) para fins de inteligência e contrainteligência. Uma típica limitação legal é a proibição de espionagem sobre cidadãos domésticos, o que estimula essas agências a cooperar internacionalmente e trocar dados. É aí que entram os acordos Five, Nine e Fourteen Eyes — os principais instrumentos legais para vigilância transfronteiriça.

Além desses blocos ocidentais, acordos similares existem em outras regiões, o que reduz drasticamente os lugares seguros para dados pessoais. Medidas extras de proteção, como criptografia forte, são essenciais.

O **Five Eyes** — Austrália, Canadá, Nova Zelândia, Reino Unido e Estados Unidos. Esses países assinaram o **Acordo UKUSA**, que formaliza a cooperação na coleta, análise e compartilhamento de dados de inteligência. Oficialmente, eles se comprometem a não se espionarem como inimigos. Porém, os documentos revelados por Edward Snowden mostraram que, na prática, alguns membros chegaram a monitorar cidadãos uns dos outros.

Para contornar leis internas que proíbem espionagem doméstica, os países do Five Eyes frequentemente coletam informações de cidadãos de outros membros e depois compartilham entre si. Isso cria uma rede de vigilância indireta, mas eficaz.

O **Nine Eyes** expande o grupo ao incluir Dinamarca, França, Holanda e Noruega.  
O **Fourteen Eyes** amplia ainda mais, incluindo Bélgica, Alemanha, Itália, Espanha e Suécia.  
Na prática, todos esses blocos compartilham inteligência, e há casos de espionagem cruzada entre membros principais e secundários.

O termo **Fourteen Eyes** designa um grupo de países que compartilham inteligência entre si: Austrália, Canadá, Nova Zelândia, Reino Unido, Estados Unidos, Dinamarca, França, Holanda, Noruega, Bélgica, Alemanha, Itália, Espanha e Suécia.

### Estrutura dos blocos

* **Five Eyes**: Austrália, Canadá, Nova Zelândia, Reino Unido, EUA.
    
* **Nine Eyes**: Five Eyes + Dinamarca, França, Holanda, Noruega.
    
* **Fourteen Eyes**: Nine Eyes + Bélgica, Alemanha, Itália, Espanha, Suécia.
    

Agências adicionais como Israel, Japão, Singapura e Coreia do Sul, atuam como terceiros com a NSA, muitos via SIGINT Seniors of the Pacific, junto a França, Índia e Tailândia

## Risco de usar serviços baseados nos EUA

Devido ao modelo legal norte-americano, serviços sediados nos EUA não são recomendados para quem busca privacidade. O governo utiliza **National Security Letters (NSLs)** acompanhadas de ordens de sigilo (**gag orders**) para obrigar empresas a fornecer acesso secreto a registros financeiros e de telecomunicações de seus clientes. Essas ordens proíbem as empresas até mesmo de comunicar a existência da solicitação.

Um caso emblemático ocorreu com a **Lavabit**, serviço de email seguro criado por Ladar Levison. Após descobrirem que Edward Snowden usava o serviço, o FBI exigiu acesso a seus registros. Como a Lavabit não mantinha logs e criptografava os emails dos usuários, não havia dados entregáveis. A resposta do FBI foi emitir uma intimação exigindo as chaves SSL do serviço — o que abriria não apenas os registros de Snowden, mas todo o tráfego em tempo real (metadados e conteúdo) de todos os clientes.

Sem alternativas, Levison entregou as chaves, encerrou a Lavabit e ainda foi acusado de descumprir a intimação, sob ameaça de prisão.

Posteriormente, Levison relançou o serviço e criou o padrão **DIME (Dark Internet Mail Environment)**, um protocolo para email com criptografia de ponta a ponta. Hoje, a Lavabit oferece serviços baseados em DIME para usuários individuais e empresas.

## O que é MLAT

**MLAT (Mutual Legal Assistance Treaty)** é um **tratado de assistência jurídica mútua** entre países.

* Permite que autoridades de um país solicitem dados, provas e informações a outro país, de forma oficial e legalizada.
    
* O pedido passa por canais diplomáticos e judiciais, normalmente via Ministério da Justiça ou equivalente.
    
* Funciona mesmo quando a empresa não tem filial no país solicitante, desde que o **país-sede da empresa** tenha acordo com quem está solicitando.
    

No caso Brasil:

* Há MLAT com EUA, grande parte da UE e outros países.
    
* Não há MLAT com alguns lugares como Suíça (parcial), Panamá, Bahamas, Rússia, Sérvia, entre outros — nesses casos, obter dados é muito mais lento ou inviável.
    

## Outros Riscos - criptografia e backdoors

A existência desses pactos cria um “denominador mínimo de privacidade”, onde legislações domésticas são contornadas por espionagem cruzada. Agências compartilham dados coletados por outras, ampliando o alcance da vigilância. Se um país aprova leis amplas de vigilância, seus efeitos alcançam diversos países membros — sua atividade digital pode estar sendo monitorada por várias agências, independente da sua localização.

E há muitas tentativas de legislações contra a privacidade, sendo alguns projetos:

* Proibir Adblocks (Alemanha)
    
* Proibir criptografia (Reino Unido, França, Alemanha)
    
* Forçar empresas a terem backdoors, fornecendo acesso livre ao todos os dados dos usuários (quase todos os países do Fourteen Eyes possuem processos semelhantes contra empresas de criptografia, apple, blackberry, telegram e outros)
    
* Semelhante à proibição de criptografia e backdoors, vários países exigem as chaves de criptografia e possuem leis que impõem a obrigação de acessar dados criptografados (via judicial) bem como permitir interceptação em tempo real e acesso ao conteúdo
    

Outros países da UE (Portugal, Espanha, por exemplo), também possuem processos judiciais, projetos de leis e ações que levaram a exigência de tais quebras.

Obviamente, países autoritários já proíbem essas tecnologias e possuem bem mais controles e leis bizarras como Belarus, Mianmar, Myanmar, Turcomenistão, Rússia, Iraque, Mongólia, China, etc… Inclusive o uso de VPNs.

Judicialmente, o Brasil já teve decisões nos tribunais a respeito destes tipos de quebra e com a suspensão do serviço perante a impossibilidade ou recusa de fornecimento dos acessos.

Este mapa ilustra a disseminação das leis que permitem aos governos acessar informações criptografadas dos usuários em todo o mundo. Enquanto alguns países exigem que as empresas de tecnologia criem recursos proativos de descriptografia, outros não impõem restrições à criptografia.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1755789409298/297ca083-e699-4a4f-9dec-6e9c9110deea.png align="center")

[https://www.theguardian.com/world/2013/nov/02/nsa-portrait-total-surveillance](https://www.theguardian.com/world/2013/nov/02/nsa-portrait-total-surveillance)

[https://cepa.org/comprehensive-reports/the-encryption-debate](https://cepa.org/comprehensive-reports/the-encryption-debate)

[https://protonvpn.com/blog/5-eyes-global-surveillance](https://protonvpn.com/blog/5-eyes-global-surveillance)

[https://www.wikiwand.com/en/articles/Five\_Eyes](https://www.wikiwand.com/en/articles/Five_Eyes)