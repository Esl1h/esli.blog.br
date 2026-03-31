---
title: "DNSCrypt, DNS Stamps e DNS Criptografado: o guia que faltava"
datePublished: 2026-03-31T12:44:31.711Z
cuid: cmnelzs6300b12dn68sjubxa3
slug: dnscrypt-dns-stamps-e-dns-criptografado-o-guia-que-faltava
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/c919b12f-b360-4162-b44e-1aa52693cf9f.jpg
tags: dns, https, tls, cryptocurrency, privacidad, dns-over-https, seguran-a-digital, dns-over-tls, dnscrypt, criptografia, dns-over-quic

---

Toda vez que você digita um endereço no navegador, seu dispositivo faz uma consulta DNS. Por padrão, essa consulta vai em texto puro para o resolver do seu provedor de internet. Aquele mesmo provedor que jura não vender seus dados mas coincidentemente exibe propaganda de coisas que você pesquisou minutos atrás.

Já falei sobre DNS e privacidade em outros artigos aqui no blog, como no [Privacidade e Segurança (2025)](https://esli.blog.br/privacidade-e-seguranca-2025), onde uso NextDNS em todos os dispositivos, e no [Como fugir das propagandas na Internet](https://esli.blog.br/como-fugir-das-propaganda-na-internet), onde mostro como o NextDNS funciona como camada de bloqueio. Neste artigo, vou aprofundar no protocolo DNSCrypt, nos DNS Stamps e no ecossistema de DNS criptografado.

## DNS tradicional: o problema

O DNS (Domain Name System) resolve nomes de domínio em endereços IP. Funciona como uma lista telefônica: você pede o número de `esli.blog` e recebe o IP do servidor. O problema é que essa "ligação" acontece sem criptografia. Qualquer pessoa no caminho (seu ISP, o administrador da rede do café, um atacante na mesma rede) consegue ver exatamente quais domínios você está resolvendo.

Não é paranoia, é superficialmente, como funciona o protocolo. O DNS foi projetado nos anos 80, quando a internet era um ambiente acadêmico e a ideia de privacidade na rede não existia.

## Os protocolos de DNS criptografado

Com o tempo, surgiram vários protocolos para resolver esse problema. Cada um com abordagens diferentes.

### DoT (DNS-over-TLS)

Encapsula as consultas DNS dentro de uma conexão TLS. Usa a porta 853, dedicada exclusivamente para esse fim. Padronizado na RFC 7858.

O fato de usar uma porta própria é uma faca de dois gumes: facilita a configuração, mas também facilita o bloqueio. Um administrador de rede (ou governo) pode simplesmente bloquear a porta 853 e acabou o DoT. O Android usa DoT nativamente na função "DNS Privado" desde a versão 9, o que popularizou o protocolo em dispositivos móveis.

### DoH (DNS-over-HTTPS)

Encapsula as consultas DNS dentro de conexões HTTPS, usando a porta 443. Padronizado na RFC 8484.

A sacada é que o tráfego DoH se mistura com o tráfego HTTPS normal. Para bloquear DoH, seria necessário bloquear todo o HTTPS, o que inviabilizaria a navegação. Isso o torna mais resistente à censura, mas também mais difícil de monitorar em ambientes corporativos (onde monitoramento pode ser legítimo). Navegadores como Firefox e Chrome suportam DoH nativamente.

### DoQ (DNS-over-QUIC)

Usa o protocolo QUIC (que por sua vez roda sobre UDP) para transportar consultas DNS. Padronizado na RFC 9250.

Mais recente, promete menor latência que DoT/DoH por eliminar o handshake TLS+TCP separado. Ainda com adoção limitada, mas providers como AdGuard DNS já suportam.

### DoH3 (DNS-over-HTTP/3)

Similar ao DoQ, mas usando HTTP/3 como transporte. Como o HTTP/3 já usa QUIC, herda as mesmas vantagens de latência. Providers como DNS0 (fundado pelos co-fundadores do NextDNS) já suportam.

### ODoH (Oblivious DNS-over-HTTPS)

Uma camada de anonimização sobre o DoH. Usa um proxy intermediário (relay) para que o resolver nunca veja o IP do cliente que fez a consulta. Padronizado na RFC 9230.

O conceito é semelhante ao Anonymized DNSCrypt (que veremos adiante): separar quem pergunta de quem responde.

### DNSCrypt

E aqui é onde a conversa fica interessante.

## DNSCrypt: o protocolo que a indústria ignorou

O DNSCrypt foi criado por Frank Denis (jedisct1), o mesmo autor do libsodium. Não é um "remendo" sobre outro protocolo. Foi projetado especificamente para proteger tráfego DNS.

O que ele faz:

**Autenticação do servidor.** O cliente valida que está falando com o resolver legítimo usando a chave pública do provider. Sem depender de CAs (Certificate Authorities). Isso elimina toda a cadeia de confiança de TLS, que é um ponto de falha conhecido.

**Criptografia da query e resposta.** Usa Curve25519 para troca de chaves e XSalsa20-Poly1305 (ou XChaCha20-Poly1305) para criptografia autenticada. Tudo baseado em criptografia de curva elíptica, sem depender de RSA ou da infraestrutura de certificados X.509.

**Anti-replay e anti-forgery.** Cada query tem um nonce único. Respostas forjadas ou repetidas são detectadas e descartadas.

**Padding.** As queries e respostas são preenchidas com dados aleatórios para dificultar análise de tráfego baseada no tamanho dos pacotes.

O protocolo roda sobre UDP (porta 443 por padrão) e opcionalmente TCP. É leve, rápido e não depende de infraestrutura de PKI.

O protocolo está em processo de padronização no IETF como Internet-Draft (draft-denis-dprive-dnscrypt-06, atualizado em abril de 2025).

### Por que a indústria ignorou?

Google, Cloudflare, Apple, Microsoft, todos adotaram DoH e DoT. O DNSCrypt ficou de fora do mainstream. Não porque seja inferior tecnicamente, mas porque não tem uma megacorp por trás patrocinando a adoção. DoH usa HTTPS, que toda a infraestrutura web já suporta. DNSCrypt exige implementação própria.

Resultado: o protocolo DNSCrypt em si tem adoção limitada entre os grandes providers. AdGuard DNS, Quad9 e CleanBrowsing suportam. NextDNS, Cloudflare e Google não suportam o protocolo DNSCrypt, apenas DoH e DoT.

Mas aqui entra a distinção crucial: **o protocolo DNSCrypt é uma coisa, o software dnscrypt-proxy é outra.**

### dnscrypt-proxy: o canivete suíço

O dnscrypt-proxy é a implementação de referência do cliente. Escrito em Go, roda em praticamente qualquer coisa: Linux, macOS, Windows, Android, roteadores com OpenWrt.

O ponto é que o dnscrypt-proxy não se limita ao protocolo DNSCrypt. Ele suporta:

*   DNSCrypt v2
    
*   DoH (DNS-over-HTTPS)
    
*   ODoH (Oblivious DoH)
    
*   Anonymized DNSCrypt
    

Então mesmo que um provider não suporte o protocolo DNSCrypt (como o NextDNS), você ainda pode usar o dnscrypt-proxy para se conectar via DoH. O software aceita stamps de qualquer protocolo suportado.

### Anonymized DNSCrypt: DNS sem rastreio

O DNS criptografado resolve o problema de interceptação no caminho. Mas não resolve o problema do próprio resolver saber quem fez a consulta. Seu IP chega ao servidor junto com a query.

O Anonymized DNSCrypt resolve isso introduzindo relays. Funciona assim:

1.  O cliente criptografa a query para o **resolver** final (usando a chave pública do resolver).
    
2.  Envia a query criptografada para um **relay**.
    
3.  O relay não tem a chave do resolver, então não consegue ler o conteúdo. Ele apenas encaminha o pacote.
    
4.  O resolver recebe a query, descriptografa, resolve e responde via relay.
    

O relay sabe o IP do cliente, mas não sabe o conteúdo da query. O resolver sabe o conteúdo da query, mas só vê o IP do relay. Nenhum dos dois tem o quadro completo.

Isso é fundamentalmente diferente de usar Tor para DNS (que é lento e não foi projetado para isso) ou de confiar cegamente que o resolver é “no-log”.

A lista de relays disponíveis está no repositório oficial: https://github.com/DNSCrypt/dnscrypt-resolvers

## DNS Stamps: o QR Code do DNS

Configurar DNS criptografado manualmente é chato. Você precisa informar protocolo, IP, porta, hostname, path (no caso de DoH), chave pública (no caso de DNSCrypt), e opcionalmente hashes de certificado. São vários campos, cada um com formato diferente dependendo do protocolo.

O DNS Stamp resolve isso codificando tudo em uma única string.

### Estrutura

Um stamp começa com `sdns://` seguido de um payload em base64url. O primeiro byte do payload identifica o protocolo:

| Byte | Protocolo |
| --- | --- |
| 0x00 | Plain DNS |
| 0x01 | DNSCrypt |
| 0x02 | DoH |
| 0x03 | DoT |
| 0x04 | DoQ |
| 0x05 | ODoH target |
| 0x81 | Anonymized DNSCrypt relay |
| 0x85 | ODoH relay |

Depois do protocolo, o payload contém propriedades (DNSSEC, no-log, no-filter), endereço IP, hashes de certificado, hostname, path e opcionalmente IPs de bootstrap.

### Exemplo prático

Um stamp DoH para o NextDNS com ID `abc123`:

```plaintext
sdns://AgMAAAAAAAAAAAAADmRucy5uZXh0ZG5zLmlvBy9hYmMxMjM
```

Decodificando:

*   Protocolo: 0x02 (DoH)
    
*   Hostname: dns.nextdns.io
    
*   Path: /abc123
    
*   No-log: sim
    
*   DNSSEC: sim
    

### Gerando stamps

O gerador oficial está em https://dnscrypt.info/stamps/ (anteriormente em https://stamps.dnscrypt.info). Você seleciona o protocolo, preenche os campos e o stamp é gerado automaticamente. Funciona no sentido inverso também: cole um stamp e ele decodifica os campos.

A especificação dos DNS Stamps está em processo de padronização no IETF (draft-denis-dns-stamps-00, julho de 2025), suportando DNSCrypt, DoH, DoT, DoQ e ODoH.

### Quem publica stamps nativamente?

Nem todo provider facilita a vida. Alguns publicam stamps prontos na documentação, outros exigem que você gere manualmente.

| Provider | Stamp na doc | Protocolos |
| --- | --- | --- |
| AdGuard DNS | Sim | DNSCrypt, DoH, DoT |
| Cloudflare | Sim (listado no dnscrypt-resolvers) | DoH |
| Quad9 | Sim (listado no dnscrypt-resolvers) | DNSCrypt, DoH |
| CleanBrowsing | Sim (listado no dnscrypt-resolvers) | DNSCrypt, DoH |
| Mullvad DNS | Sim | DoH |
| Control D | Sim | DoH, DoT |
| DNS0 | Sim | DoH, DoQ |
| NextDNS | Sim (Setup > Linux > DNSCrypt) | DoH (via stamp) |

**Correção importante sobre o NextDNS:** diferente do que muitos pensam, o NextDNS **fornece** o stamp na página de Setup. Vá em https://my.nextdns.io, acesse Setup Guide, selecione Routers, e procure a seção DNSCrypt. O stamp já está gerado com seu config ID. Ele é um stamp DoH (não DNSCrypt nativo, pois o NextDNS não suporta o protocolo DNSCrypt).

Se preferir gerar manualmente ou adicionar um nome de dispositivo ao path, use o gerador em https://dnscrypt.info/stamps/ com os seguintes campos:

| Campo | Valor |
| --- | --- |
| Protocol | DNS-over-HTTPS |
| IP Address | (vazio) |
| Host name | dns.nextdns.io |
| Hashes | (vazio) |
| Path | /SEU\_ID (ex: /abc123) ou /SEU\_ID/nome-dispositivo |
| No logs | marcado |
| No filter | desmarcado |
| DNSSEC | marcado |

O path deve começar com `/`. Se o ID é `abc123`, o path é `/abc123`. Para identificar o dispositivo no painel, adicione um nome: `/abc123/fedora-desktop`.

A lista completa de resolvers com stamps prontos está no repositório oficial: https://github.com/DNSCrypt/dnscrypt-resolvers

O dnscrypt-proxy baixa essas listas automaticamente e permite selecionar servidores por nome, protocolo, localização, ou filtrar por propriedades como no-log e DNSSEC.

## Resumo dos protocolos

| Protocolo | Porta | Transporte | Resistência a censura | Depende de CA | Anonimização nativa |
| --- | --- | --- | --- | --- | --- |
| Do53 (tradicional) | 53 | UDP/TCP | Nenhuma | Não | Não |
| DoT | 853 | TLS | Baixa (porta dedicada) | Sim | Não |
| DoH | 443 | HTTPS | Alta (mistura com HTTPS) | Sim | Não (ODoH sim) |
| DoQ | 853/8853 | QUIC | Média | Sim | Não |
| DNSCrypt | 443\* | UDP/TCP | Alta | Não (chave própria) | Sim (via relay) |

\* Porta padrão 443, mas configurável.

## Próximos artigos

Este artigo é o primeiro de uma série de três. No [próximo artigo](https://esli.blog.br/dnscrypt-proxy-linux-nextdns), mostro como instalar e configurar o dnscrypt-proxy no Linux (Fedora e Arch) usando o NextDNS como resolver. No [terceiro](https://esli.blog.br/dnscrypt-android-invizible-pro-nextdns), a configuração no Android com o InviZible Pro e alternativas.

Se você ainda está usando o DNS do seu provedor, considere que ele provavelmente sabe mais sobre seus hábitos de navegação do que você gostaria. E se pudesse cobrar por cada consulta que bisbilhota, já teria financiado uma ida à lua.

* * *

Outros artigos relacionados no blog:

*   [Privacidade e Segurança (2025)](https://esli.blog.br/privacidade-e-seguranca-2025)
    
*   [VPN não é o suficiente](https://esli.blog.br/vpn-nao-e-o-suficiente)
    
*   [Como fugir das propagandas na Internet](https://esli.blog.br/como-fugir-das-propaganda-na-internet)
    
*   [Qual melhor navegador](https://esli.blog.br/qual-melhor-navegador)
    
*   [Guia de Comandos Linux para Redes (modelo OSI)](https://esli.blog.br/comandos-linux-para-analise-de-redes-guia-completo-por-camada-da-tabela-osi)
    

Referências:

*   https://dnscrypt.info/
    
*   https://dnscrypt.info/stamps-specifications/
    
*   https://github.com/DNSCrypt/dnscrypt-proxy
    
*   https://github.com/DNSCrypt/dnscrypt-resolvers
    
*   https://www.ietf.org/archive/id/draft-denis-dns-stamps-00.html
    
*   https://www.cloudflare.com/learning/dns/dns-over-tls/