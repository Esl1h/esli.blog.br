---
title: "Apps e os tipos de redes de comunicação"
datePublished: Tue Apr 26 2022 22:18:49 GMT+0000 (Coordinated Universal Time)
cuid: cl2gpmxm301whz0nv93fq45dq
slug: apps-e-os-tipos-de-redes-de-comunicacao
cover: https://cdn.hashnode.com/res/hashnode/image/unsplash/ZiQkhI7417A/upload/v1651009068225/k1MS8BrKE.jpeg
tags: network, apps, social, messenger, communication

---

# Tipos de redes de comunicação

Existem várias arquiteturas de rede comumente usadas para retransmitir mensagens entre usuários. 
Essas redes podem fornecer diferentes garantias de privacidade, e é por isso que vale a pena considerar seu modelo de ameaça (Threat modeling) ao tomar uma decisão sobre qual aplicativo usar.

O objetivo da modelagem de ameaças é fornecer aos defensores uma análise sistemática de quais controles ou defesas precisam ser incluídos, dada a natureza do sistema, o perfil do provável invasor, os vetores de ataque mais prováveis ​​e os ativos mais desejados por um invasor. 
A modelagem de ameaças responde a perguntas como “Onde estou mais vulnerável a ataques?”, “Quais são as ameaças mais relevantes?” e “O que preciso fazer para me proteger contra essas ameaças?”.



## Redes centralizadas



![network-centralized.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651008836057/mcweuT78F.png)

Os aplicativos centralizados são aqueles em que todos estão no mesmo servidor ou rede de servidores controlados pela mesma organização.

Alguns podem ser auto-hospedados, ou seja, permitem que você configure seu próprio servidor. 
A auto-hospedagem pode fornecer garantias adicionais de privacidade, como ausência de logs de uso ou acesso limitado a metadados (dados sobre quem está usando). 

Os apps centralizados auto-hospedados são isolados e todos devem estar no mesmo servidor para se comunicar.


**Vantagens**

- Novos recursos e alterações podem ser implementados mais rapidamente.
- Mais fácil de começar e encontrar contatos.
- Ecossistemas de recursos mais maduros e estáveis, pois são mais fáceis de programar em um software centralizado.
- Os problemas de privacidade podem ser reduzidos quando você confia em um servidor que está auto-hospedando.

**Desvantagens**

- Pode incluir controle ou acesso restrito. Isso pode incluir coisas como:
Ser proibido de conectar clientes de terceiros à rede centralizada que possam proporcionar maior personalização ou melhor experiência do usuário. Frequentemente definido nos Termos e Condições de uso.
- Pouca ou nenhuma documentação para desenvolvedores de terceiros.
- A propriedade , a política de privacidade e as operações do serviço podem mudar facilmente quando uma única entidade o controla, comprometendo potencialmente o serviço posteriormente.
- A auto-hospedagem requer esforço e conhecimento de como configurar um serviço.



## Redes federadas (ou descentralizadas)



![network-decentralized.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651008858768/w9ydlNIK9.png)

Os apps federados usam vários servidores independentes e descentralizados que podem conversar entre si (o e-mail é um exemplo de serviço federado). 

A federação permite que os administradores de sistema controlem seu próprio servidor e ainda façam parte da rede de comunicações maior.

Quando auto-hospedado, os usuários de um servidor federado podem descobrir e se comunicar com usuários de outros servidores, embora alguns servidores possam optar por permanecer privados por não serem federados (por exemplo, servidor de equipes de trabalho).

**Vantagens**

- Permite maior controle sobre seus próprios dados ao executar seu próprio servidor.
- Permite que você escolha em quem confiar seus dados, escolhendo entre vários servidores "públicos".
- Geralmente permite clientes de terceiros que podem fornecer uma experiência mais nativa, personalizada ou acessível.
- O software do servidor pode ser verificado se corresponde ao código-fonte público, supondo que você tenha acesso ao servidor ou confie na pessoa que o faz 


**Desvantagens**

- Adicionar novos recursos é mais complexo, pois esses recursos precisam ser padronizados e testados para garantir que funcionem com todos os servidores da rede.
- Devido ao ponto anterior, os recursos podem estar ausentes, incompletos ou funcionando de maneiras inesperadas em comparação com plataformas centralizadas, como retransmissão de mensagens quando offline ou exclusão de mensagens.
- Alguns metadados podem estar disponíveis (por exemplo, informações como "quem está falando com quem", mas não o conteúdo real da mensagem se a End-to-end encryption for usado).
- Servidores federados geralmente exigem confiança no administrador do seu servidor. - Eles podem ser um hobby ou não possuir um "profissional de segurança" e podem não fornecer documentos padrão como uma política de privacidade ou termos de serviço detalhando como seus dados são utilizados.
- Os administradores de servidor às vezes optam por bloquear outros servidores, que são uma fonte de abuso não moderado ou quebram as regras gerais de comportamento aceito. Isso prejudicará sua capacidade de se comunicar com usuários nesses servidores.


## Redes ponto a ponto (ou distribuidas)


![network-distributed.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651008871528/YAGFkw2Ro.png)


Os apps P2P se conectam a uma rede distribuída de nós para retransmitir uma mensagem ao destinatário sem um servidor de terceiros.

Os clientes (peers) geralmente se encontram por meio do uso de uma rede de computação distribuída . Exemplos disso incluem Distributed Hash Tables (DHT), usado por torrents e IPFS, por exemplo. 

Outra abordagem são as redes baseadas em proximidade, onde uma conexão é estabelecida por WiFi ou Bluetooth (por exemplo, Briar ou o protocolo de rede social Scuttlebutt).

Uma vez que um peer tenha encontrado uma rota para seu contato por meio de qualquer um desses métodos, uma conexão direta entre eles é feita. Embora as mensagens sejam geralmente criptografadas, um observador ainda pode deduzir a localização e a identidade do remetente e do destinatário.

As redes P2P não usam servidores, pois os usuários se comunicam diretamente entre si e, portanto, não podem ser auto-hospedados. No entanto, alguns serviços adicionais podem contar com servidores centralizados, como descoberta de usuários ou retransmissão de mensagens offline, que podem se beneficiar da auto-hospedagem.

**Vantagens**

- Informações mínimas são expostas a terceiros.
- As plataformas P2P modernas implementam o encriptação end-to-end (E2EE) por padrão. Não há servidores que possam interceptar e descriptografar suas transmissões, ao contrário dos modelos centralizados e federados.

**Desvantagens**

Conjunto de recursos reduzido:
- As mensagens só podem ser enviadas quando ambos os peers estiverem online, no entanto, seu cliente pode armazenar mensagens localmente para aguardar o retorno do contato online.
- Geralmente aumenta o uso da bateria em dispositivos móveis, pois o cliente deve permanecer conectado à rede distribuída para saber quem está online.
- Alguns recursos comuns do messenger podem não ser implementados ou estar incompletos, como a exclusão de mensagens.
- Seu endereço IP e o dos contatos com os quais você está se comunicando podem ser expostos se você não usar o software em conjunto com uma VPN ou rede independente , como Tor ou I2P . Muitos países têm alguma forma de vigilância em massa e/ou retenção de metadados.



## Roteamento anônimo



![network-anonymous-routing.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1651008884353/5MXg-WPD7.png)


Um mensageiro que usa roteamento anônimo oculta a identidade do remetente, do destinatário ou evidência de que eles estão se comunicando. Idealmente, um mensageiro deve ocultar todos os três.

Há muitas maneiras diferentes de implementar o roteamento anônimo. Um dos mais famosos é o onion routing (ou seja , Tor), que comunica mensagens criptografadas por meio de uma rede de sobreposição virtual que oculta a localização de cada nó, bem como o destinatário e o remetente de cada mensagem. 

O remetente e o destinatário nunca interagem diretamente, e apenas se encontram por meio de um nó de encontro secreto, para que não haja vazamento de endereços IP nem localização física. 
Os nós não podem descriptografar mensagens nem o destino final, apenas o destinatário pode. Cada nó intermediário só pode descriptografar uma parte que indica para onde enviar a próxima mensagem ainda criptografada, até chegar ao destinatário que pode descriptografá-la totalmente, daí as "camadas de cebola" (onion layers).

A auto-hospedagem de um nó em uma rede de roteamento anônima não fornece ao hoster benefícios adicionais de privacidade, mas contribui para a resiliência de toda a rede contra ataques de identificação para benefício de todos.

**Vantagens**

- Mínima ou nenhuma informação é exposta a outras partes.
- As mensagens podem ser retransmitidas de forma descentralizada, mesmo que uma das partes esteja offline.

**Desvantagens**

- Propagação lenta da mensagem.
- Geralmente limitado a menos tipos de mídia, principalmente texto, pois a rede é lenta.
- Menos confiável se os nós forem selecionados por roteamento aleatório, alguns nós podem estar muito distantes do remetente e do receptor, adicionando latência ou até mesmo deixando de transmitir mensagens se um dos nós ficar offline.
- Mais complexo para começar, pois é necessária a criação e o backup seguro de uma chave privada criptográfica.
- Assim como outras plataformas descentralizadas, adicionar recursos é mais complexo para os desenvolvedores do que em uma plataforma centralizada, portanto, os recursos podem estar ausentes ou implementados de forma incompleta, como retransmissão de mensagens offline ou exclusão de mensagens.


referência: https://www.privacyguides.org/