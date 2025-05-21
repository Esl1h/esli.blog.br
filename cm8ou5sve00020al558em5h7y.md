---
title: "Snowflake: Proxy Temporário para Contornar a Censura na Internet"
seoTitle: "Bypass Internet Censorship with Snowflake Proxy"
seoDescription: "Snowflake by Tor provides temporary proxies via WebRTC to bypass internet censorship and access information in restricted environments"
datePublished: Tue Mar 25 2025 18:35:16 GMT+0000 (Coordinated Universal Time)
cuid: cm8ou5sve00020al558em5h7y
slug: snowflake-para-contornar-a-censura-na-internet
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/5AiWn2U10cw/upload/46661bae48e8a36cf54ac356a13163e3.jpeg
tags: snowflake, proxy, tor, brave, brave-browser, censorship, censura

---

A censura na internet é uma realidade em muitos países, onde governos e entidades restringem o acesso a informações e comunicação livre. Para combater essas restrições, o Projeto Tor desenvolveu o **Snowflake**, uma ferramenta que funciona como um proxy temporário baseado no protocolo WebRTC, permitindo que usuários acessem a rede Tor mesmo em ambientes altamente censurados.

## Como Funciona o Snowflake?

O Snowflake utiliza um sistema distribuído de proxies voluntários para disfarçar o tráfego da rede Tor, tornando-o semelhante a um tráfego comum de WebRTC (usado em chamadas de vídeo e voz). Esse disfarce dificulta a detecção e bloqueio por sistemas de censura.

### Arquitetura e Fluxo de Conexão

1. **Usuário censurado**: Inicia uma conexão com a rede Tor utilizando o Snowflake.
    
2. **Sistema de correspondência**: O Snowflake conecta o usuário a um proxy voluntário ativo.
    
3. **Proxy voluntário**: Encaminha o tráfego do usuário para um Bridge (ponto de entrada Tor não listado).
    
4. **Bridge Tor**: Redireciona a conexão para a rede Tor, permitindo acesso livre e anônimo.
    

### Características do Snowflake

* **Uso de proxies temporários**: A conexão é roteada por vários proxies voluntários, dificultando o rastreamento.
    
* **Protocolo WebRTC**: Evita bloqueios baseados em padrões de tráfego tradicionais da rede Tor.
    
* **Anonimato**: O endereço IP do usuário não é exposto, apenas o IP do nó de saída do Tor é visível para o destino final.
    

## Benefícios do Snowflake

* **Acesso irrestrito à informação**: Possibilita que pessoas em países com censura acessem conteúdo bloqueado.
    
* **Fácil adesão para voluntários**: Não é necessário infraestrutura complexa para rodar um proxy Snowflake.
    
* **Privacidade aprimorada**: O uso do Tor combinado com proxies temporários reduz a exposição de metadados.
    

## Como Contribuir com o Snowflake

Qualquer pessoa pode ajudar o Snowflake ao rodar um proxy voluntário. Existem duas formas principais de contribuir:

### Extensão para Navegador

Usuários do Firefox ou Chrome podem instalar a extensão do Snowflake, que automaticamente transforma o navegador em um proxy voluntário. Isso permite que pessoas em países censurados usem sua conexão para acessar a rede Tor.

* [Extensão Snowflake para Chrome](https://chromewebstore.google.com/detail/snowflake/mafpmfcccpbjnhfhjnllmmalhifmlcie)
    

### Snowflake no Brave Browser

No Brave, acesse brave://settings/privacy e habilite nas configurações do Tor

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742926757236/4c38cea1-0302-49bd-9b8d-d06bfb6977a4.png align="center")

Ao habilitar, a extensão será ativada (não precisa baixar na Store);

Será aberto uma tela do Snowflake e será necessário clicar em “Concordo” para ativar.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742927143497/ddc3dd21-2953-40c3-9d68-9e45455a40d0.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1742926962351/81810678-ee2b-471c-8853-4f9aa5040eb3.png align="center")

### Executando um Proxy Independente

Usuários mais avançados podem configurar proxies Snowflake diretamente em servidores VPS ou rússia, garantindo um ponto de acesso confiável para a rede Tor. O guia de instalação oficial do Projeto Tor pode ser encontrado aqui:

* [Guia de configuração do Snowflake](https://community.torproject.org/relay/setup/snowflake/)
    

## Testando a Conectividade do Snowflake

Se você deseja verificar se sua rede permite conexões via Snowflake, utilize a ferramenta OONI Probe:

* [Teste OONI Probe Tor Snowflake](https://ooni.org/nettest/tor-snowflake/)
    

## Há algum risco em ativar o Snowflake?  

Nem o Brave Browser nem o Projeto Tor têm conhecimento de quaisquer riscos graves associados à instalação da extensão Snowflake. O seu endereço IP não será publicado em lado nenhum e só será utilizado para ligar pessoas ao seu computador para acederem à rede Tor.

Além disso, seu computador não se conectará diretamente a sites em nome de outra pessoa. Em vez disso, o seu computador passará mensagens encriptadas entre os usuários ligados e outros computadores na rede Tor.

A extensão não registra a identidade dos usuários que utilizaram a sua conexão.

Apenas as estatísticas agregadas são visíveis no painel da extensão.

### Quem não deve executar o Snowflake?

Não deve ativar esta opção se:

Vive num país onde o Tor está bloqueado ou onde a evasão à censura é ilegal

Utiliza um computador compartilhado (por exemplo, no seu local de trabalho, a menos que tenha a permissão do seu empregador)

Tem um plano de dados pequeno ou uma ligação à Internet lenta

Se vive num país censurado, em vez de executar a extensão Snowflake, ligue-se à rede Tor.

### Que dados serão partilhados com o Projeto Tor?

Apenas a informação técnica necessária para permitir que o Projeto Tor ligue outro usuário ao seu computador será partilhada com o serviço gerido pelo Projeto Tor.

Essa informação, que inclui o seu endereço IP, será também utilizada para ligar outros usuários à rede Tor utilizando a sua ligação à Internet. Nenhuma destas informações é armazenada, apenas gerado estatísticas presente em [https://metrics.torproject.org/collector/recent/snowflakes/ .](https://metrics.torproject.org/collector/recent/snowflakes/)

## Conclusão

O Snowflake é uma solução inovadora para contornar bloqueios na internet e garantir acesso livre à informação. Seja como usuário ou como voluntário, qualquer pessoa pode contribuir para uma internet mais aberta e acessível. Com o Snowflake, o Projeto Tor continua sua missão de fornecer anonimato e resistência à censura em um mundo cada vez mais vigiado.

## Links

* Site do Snowflake: [https://snowflake.torproject.org/](https://snowflake.torproject.org/)
    
* Guia de instalação do Snowflake: [https://community.torproject.org/relay/setup/snowflake/](https://community.torproject.org/relay/setup/snowflake/)
    
* Teste OONI Probe Tor Snowflake: [https://ooni.org/nettest/tor-snowflake/](https://ooni.org/nettest/tor-snowflake/)
    

# Texto de consentimento da extensão

## Obrigado por instalar a extensão da web Snowflake!

Você sabia que os proxies Snowflake são operados totalmente por voluntários? Em outras palavras, um usuário é combinado com um proxy voluntário aleatório do Snowflake, executado por um voluntário como você! Então, se você quiser ajudar as pessoas a contornar a censura, considere instalar e executar um proxy Snowflake. O único pré-requisito é que a Internet em seu país já **não** seja fortemente censurada.

Você pode se juntar a milhares de voluntários de todo o mundo que possuem um proxy Snowflake instalado e em execução. Não há necessidade de se preocupar com quais sites as pessoas estão acessando por meio de seu proxy Snowflake. O endereço IP de navegação visível deles corresponderá ao nó de saída do Tor, não ao seu.

Para fornecer funcionalidade básica, os proxies Snowflake (complemento que você instalou em seu navegador) enviam alguns metadados ao corretor Snowflake sobre a configuração de rede do proxy. Esses metadados são, exaustivamente: o endereço IP do proxy (seu endereço IP), o tipo de NAT relatado, a versão do protocolo snowflake que você (proxy) está executando, o fato de que o proxy (complemento que você instalou) é uma extensão web e o número de clientes já conectados ao proxy (complemento que você instalou). Esses dados são utilizados apenas para o estabelecimento da conexão e agregação estática e não são armazenados na corretora. Consulte nossa [política de privacidade](https://addons.mozilla.org/en-US/firefox/addon/torproject-snowflake/privacy) para obter mais detalhes sobre as informações que coletamos e exibimos. Quando estiver pronto, você pode permitir a ativação do proxy Snowflake clicando no botão abaixo.

Este complemento transmite apenas a quantidade mínima de dados necessária para funcionar como um proxy Snowflake. Se você não se sentir confortável com a transmissão dos dados enumerados acima ou com qualquer parte de nossa política de privacidade, recomendamos que você remova o complemento Snowflake.

## Relatando bugs

Se você encontrar problemas com o Snowflake - esteja usando-o ou executando-o -, considere preencher um relatório de bug. Há duas maneiras de enviar um relatório de erro:

1. [Solicite uma conta](https://gitlab.onionize.space/) no GitLab do Projeto Tor, e então [abra um novo chamado](https://gitlab.torproject.org/tpo/anti-censorship/pluggable-transports/snowflake/-/issues) no projeto Snowflake.
    
2. Protocole um [ticket anônimo](https://anonticket.onionize.space/) gerando um identificador e logando com ele. Depois ache o projeto Snowflake na **Lista de projetos** e depois crie uma nova questão.
    

Por favor, tente ser o mais descritivo possível em seu chamado e, se possível, inclua mensagens de log que nos ajudarão a reproduzir o bug.

## Buscando suporte para usar o Snowflake

Se você teve problemas quando se conectando ao Tor usando o Snowflake, você pode entrar em contato com o canal de suporte do Tor pelo [Telegram](https://t.me/TorProjectSupportBot). Você também pode navegar no [Portal De Suporte do Tor](https://support.torproject.org/censorship/) e no [Tor Forum](https://forum.torproject.org/tag/snowflake) para achar respostas.