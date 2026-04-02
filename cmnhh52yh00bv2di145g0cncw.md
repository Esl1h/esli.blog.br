---
title: "SNI Leak: o calcanhar de Aquiles do DNS seguro"
datePublished: 2026-04-02T12:51:59.421Z
cuid: cmnhh52yh00bv2di145g0cncw
slug: sni-leak-o-calcanhar-de-aquiles-do-dns-seguro
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/9cefcb8e-d4d6-44ef-b12a-a3e859476b92.jpg
tags: dns, https, hacking, tls, leaks, dns-over-https, tcp-handshake, dns-over-tls, dnscrypt, dns-over-quic, sni

---

Você configurou DNSCrypt. Ativou DoH. Trocou o DNS do provedor por um que respeita privacidade. Fez tudo certo. E mesmo assim, o seu provedor de internet pode continuar sabendo exatamente quais sites você acessa.

Bem-vindo ao problema do SNI.

Se você acompanhou a série sobre [DNSCrypt](https://esli.blog.br/dnscrypt-dns-stamps-e-dns-criptografado-o-guia-que-faltava) e o artigo sobre [por que não confiar no seu provedor de internet](https://esli.blog.br/nao-confie-no-seu-provedor-de-internet), já sabe que criptografar as consultas DNS é o primeiro passo para recuperar privacidade na rede. Mas DNS é só metade da equação: outra metade acontece no TLS handshake, e é ali que mora o vazamento que quase ninguém discute.

## O que é o SNI e por que ele existe

SNI significa Server Name Indication. É um campo enviado pelo cliente (seu navegador) no início de uma conexão TLS, antes de qualquer criptografia do conteúdo.

O motivo é prático: quando múltiplos sites compartilham o mesmo IP (o que é extremamente comum em hospedagens compartilhadas, CDNs e plataformas cloud), o servidor precisa saber qual certificado TLS apresentar. O cliente informa isso enviando o hostname de destino no ClientHello, a primeira mensagem do handshake TLS. Em texto claro.

Isso significa que qualquer entidade posicionada entre você e o servidor de destino (ISP, operador de rede corporativa, governo, Wi-Fi do aeroporto) consegue ler o campo SNI e saber exatamente qual domínio você está acessando.

Resumindo: HTTPS protege o conteúdo. O SNI entrega o destino.

## O que o atacante vê (e o que ele faz com isso)

Com acesso ao SNI, um observador de rede consegue:

**Construir um perfil completo de navegação.** Mesmo sem ver o conteúdo das páginas, saber que alguém acessou `consultamedica.com.br`, `vagas.com` e `advogadotrabalhista.com.br` no mesmo dia já conta uma história.

**Bloquear sites seletivamente.** A Coreia do Sul fez exatamente isso em 2019: depois que os usuários começaram a contornar bloqueios DNS, o governo instruiu os ISPs a inspecionar o campo SNI nos handshakes TLS para derrubar conexões a domínios proibidos. O DNS criptografado virou inútil. O SNI era o novo ponto de controle.

**Censurar sem quebrar criptografia.** O ISP não precisa decifrar o tráfego HTTPS. Basta ler o SNI, comparar com uma lista e dropar o pacote SYN ou enviar um RST. É trivial de implementar em DPI (Deep Packet Inspection) e invisível para quem não está monitorando.

**Correlacionar tráfego com identidade.** Combinando SNI com IP de origem e timestamps, é possível vincular padrões de acesso a um assinante específico. Isso é feito sistematicamente em regimes autoritários e, como demonstramos no artigo anterior, por ISPs em democracias que retêm logs por obrigação legal.

O DNS cifrado resolve a pergunta "para qual IP este domínio aponta?". O SNI vaza a resposta: "qual domínio estou visitando?". São dois canais de metadados diferentes. Proteger um e ignorar o outro é como trancar a porta da frente e deixar a janela aberta.

## A evolução: do ESNI ao ECH

A comunidade técnica não ignorou o problema. A primeira tentativa de solução foi o ESNI (Encrypted Server Name Indication), proposto em 2018 como extensão experimental do TLS 1.3. A ideia era simples: criptografar o campo SNI usando uma chave pública disponível via DNS.

O ESNI funcionou como prova de conceito, mas tinha limitações práticas. A criptografia era aplicada somente ao SNI, deixando outros campos do ClientHello (como ALPN, lista de cipher suites, e extensões) ainda visíveis. Isso permitia que observadores inferissem informações do destino mesmo sem o SNI explícito. O ESNI também dependia de registros DNS TXT específicos, o que tornava a distribuição de chaves frágil e difícil de escalar.

A resposta foi o ECH (Encrypted Client Hello), que substituiu o ESNI e está em processo de padronização final no IETF (draft 25 no momento desta publicação). A diferença fundamental: em vez de criptografar apenas o SNI, o ECH criptografa o ClientHello inteiro.

## Como o ECH funciona

O mecanismo do ECH divide o ClientHello em duas partes:

**Outer ClientHello** (público): Contém informações genéricas como versão do TLS, cipher suites e um SNI genérico. No caso do Cloudflare, todas as conexões ECH mostram `cloudflare-ech.com` como SNI externo. O observador de rede vê apenas que você está acessando "algo no Cloudflare", junto com milhões de outros usuários.

**Inner ClientHello** (cifrado): Contém o SNI real, as extensões sensíveis e todas as informações que antes eram enviadas em texto claro. Esse bloco é criptografado usando uma chave pública (ECHConfig) que o navegador obtém via registro HTTPS/SVCB no DNS.

Para o ISP ou qualquer intermediário, a conexão TLS parece ser para `cloudflare-ech.com`. O destino real só é revelado dentro do túnel criptografado, acessível apenas pelo servidor de destino (ou pela CDN que o hospeda).

Pense assim: sem ECH, é como enviar uma carta com o nome do destinatário escrito no envelope. Com ECH, o envelope mostra "Cloudflare" e o nome real está dentro, lacrado.

## Dependência do DNS criptografado

Aqui está o ponto que liga tudo: o ECH depende de registros DNS para funcionar. O navegador precisa buscar o registro HTTPS/SVCB do domínio para obter a ECHConfig (a chave pública de criptografia). Se essa consulta DNS for feita em texto claro, um atacante pode simplesmente remover ou modificar o registro para impedir o ECH.

Sem DoH, DoT ou DNSCrypt, o ECH pode ser sabotado antes de começar.

É por isso que a série sobre DNSCrypt não é "só sobre DNS". Ela é a fundação para que o ECH funcione. Um não substitui o outro. Eles se complementam.

A cadeia completa para privacidade real é:

1.  **DNS criptografado** (DNSCrypt, DoH, DoT) impede que o ISP saiba quais domínios você resolve
    
2.  **ECH** impede que o ISP saiba quais domínios você acessa via TLS
    
3.  **HTTPS** impede que o ISP leia o conteúdo da comunicação
    

Remova qualquer uma dessas camadas e a proteção degrada. As três juntas eliminam os metadados de navegação visíveis para observadores de rede.

## Suporte atual: quem já implementou

### Navegadores

| Navegador | ECH habilitado por padrão | Desde quando |
| --- | --- | --- |
| Firefox | Sim | Versão 119 (out/2023) |
| Chrome | Sim | Versão 117+ (rollout gradual) |
| Edge | Sim | Segue Chromium |
| Brave | Sim | Segue Chromium |
| Safari | Sim | macOS Sequoia / iOS 18+ |
| Tor Browser | Sim | Baseado no Firefox ESR |
| Mullvad Browser | Sim | Baseado no Firefox |

O Firefox foi o primeiro a ativar ECH por padrão, e exige que o DoH esteja habilitado para que o ECH funcione (o que faz sentido técnico). O Chrome e derivados fazem rollout progressivo.

### Servidores/CDNs

O Cloudflare é o principal motor de adoção do ECH no lado servidor. Ativou ECH em todos os planos, incluindo o gratuito (onde não pode ser desabilitado). Isso é relevante porque uma parte significativa dos sites da internet usa Cloudflare como CDN/proxy reverso.

Dados de 2026: aproximadamente 4,2% dos top 100K sites e 9,2% do top 1M suportam ECH. Parece pouco, mas considerando que o Cloudflare sozinho cobre uma fatia enorme do tráfego web, o impacto prático é maior do que os números sugerem.

Outros CDNs e provedores de hospedagem ainda estão avaliando. Sem suporte do servidor, o ECH simplesmente não é negociado e a conexão cai no comportamento antigo (SNI em texto claro).

## Como verificar e configurar no Linux

### Firefox

O ECH já está habilitado por padrão no Firefox 119+.

Para confirmar:

1.  Acesse `about:config`
    
2.  Busque `network.dns.echconfig.enabled` → deve estar `true`
    
3.  Busque `network.dns.http3_echconfig.enabled` → deve estar `true`
    

O ECH no Firefox depende do DoH estar ativo. Verifique em: `Configurações → Privacidade e Segurança → DNS sobre HTTPS`

Ative com Proteção Máxima e selecione um provedor (Cloudflare, NextDNS ou customizado).

### Chromium/Brave/Edge

No Chrome e derivados, o ECH era controlado pela flag até a versão 117

```plaintext
chrome://flags/#encrypted-client-hello
```

Atualmente, se o DoH está ativo e o servidor suporta ECH, o Chrome negocia automaticamente e não existe a opção de ser desativado.

Novamente, assim como no Firefox, o ECH depende de Secure DNS estar ativo. Verifique em: `Configurações → Privacidade e segurança → Segurança → Usar DNS seguro`

### Checando sites com comando dig

```plaintext
dig HTTPS crypto.cloudflare.com +short
```

Se retornar campo com `ech=`, o site suporta ECH.

A presença de ECHConfig no registro DNS tipo HTTPS é condição necessária e suficiente para afirmar que o site suporta ECH. O browser negocia se quiser - mas o suporte é do servidor.

### Validação

Para verificar se o ECH está funcionando na sua conexão:

1.  Acesse [https://crypto.cloudflare.com/cdn-cgi/trace](https://crypto.cloudflare.com/cdn-cgi/trace) e procure o campo `sni=encrypted`
    
2.  Acesse [https://defo.ie/ech-check.php](https://defo.ie/ech-check.php) para um teste mais detalhado
    

## O que o ECH não resolve

O ECH criptografa o ClientHello, isso é significativo mas não é o fim da história.

**IP de destino.** O ECH esconde o domínio, não o IP. Se um servidor hospeda um único site, o IP já revela o destino. O ECH é mais eficaz quando muitos sites compartilham a mesma infraestrutura (como em CDNs).

**Análise de tráfego.** O volume de dados, padrões de acesso e timing ainda são observáveis. Técnicas de traffic fingerprinting podem inferir o site visitado mesmo sem SNI.

**DNS não criptografado.** Se o DNS vazar o domínio antes do ECH atuar, a proteção é nula. O ECH precisa de DoH/DoT/DNSCrypt para funcionar como planejado.

**Servidores sem suporte.** Se o site que você acessa não suporta ECH, o SNI continua em texto claro. A proteção depende da adoção do lado servidor.

Para uma cobertura mais completa, combine ECH com VPN. Conforme discutido no artigo sobre [qual a melhor VPN](https://esli.blog.br/qual-melhor-vpn), uma VPN oculta o IP de destino e encapsula todo o tráfego, enquanto o ECH protege os metadados TLS. VPN + ECH + DNS criptografado é a combinação mais robusta disponível hoje sem utilizar o Tor.

## ECH como ferramenta de ataque

Vale registrar o outro lado: o ECH também é útil para atacantes. Em 2024, pesquisadores da NTT Security Holdings identificaram um malware (batizado de ECHidna) que usava ECH para esconder comunicação com servidores de comando e controle (C2). O tráfego malicioso se passava por conexões legítimas a infraestrutura CDN, tornando a detecção por firewalls tradicionais ineficaz.

Isso não é argumento contra o ECH, ao contrário: mostra que a tecnologia e a criptografia funcionam, o que muda é a intenção de uso.

## Escondendo SNI do seu provedor e de atacantes

[**zapret**](https://github.com/bol-van/zapret) e [**SpoofDPI**](https://github.com/xvzc/SpoofDPI) são ferramentas que operam na camada de rede para impedir que sistemas de Deep Packet Inspection (DPI) consigam ler o campo SNI no ClientHello do TLS.

No Linux, o **zapret** usa nfqueue (via iptables/nftables) para interceptar pacotes de saída antes de deixarem a máquina — ao capturar o ClientHello, ele aplica técnicas como fragmentação TCP (split2), manipulação de TTL e inserção de pacotes falsos (fake), fazendo com que o DPI do ISP receba os fragmentos fora de ordem ou com dados inválidos enquanto o servidor de destino remonta corretamente.

Já o **SpoofDPI** atua como proxy HTTP/HTTPS local: o navegador conecta nele, e ele estabelece a conexão real com o destino fragmentando o primeiro pacote TLS no nível da aplicação — mais simples de configurar (basta apontar o proxy do browser para 127.0.0.1:8080), porém menos flexível que o zapret. Ambos **não criptografam nem removem o SNI** — apenas tornam a inspeção passiva inviável ao quebrar as heurísticas dos equipamentos de DPI.

No Android, as principais opções são o [**InviZible Pro**](https://github.com/Gedsh/InviZible), [**ByeDPI Android**](https://github.com/dovecoteescapee/ByeDPIAndroid) e [**DPI Tunnel**](https://github.com/nomoresat/DPITunnel-android) — todos funcionam sem root criando uma VPN local no dispositivo que intercepta o tráfego de saída e aplica técnicas de fragmentação/dessincronização no ClientHello TLS antes que ele chegue ao ISP, impedindo que o DPI passivo consiga ler o SNI.

O **InviZible Pro** é o mais completo, integrando DNSCrypt, Tor e módulos anti-DPI em um único app (com root, ele injeta regras iptables diretas, dispensando a VPN local e evitando conflitos). Possui opção para falsificar o SNI.

O **ByeDPI Android** é o mais simples e focado: instala, escolhe o método de desync (split/disorder) e ativa — sem configuração extra. Assim como no Linux, **nenhum deles criptografa ou remove o SNI**, apenas fragmentam o pacote TCP para que equipamentos de inspeção passiva não consigam remontar e ler o campo em tempo real.

## Conclusão: o elo que faltava

Na prática, do lado do usuário final, o ECH é quase inteiramente dependente do navegador e do servidor (qualquer fork do Chrome/Chromium atualizado, e qualquer Firefox desde que validado o `true` nas configs).  
O que o usuário controla é diretamente limitado, mas já está no artigo:

*   Habilitar DoH/DoT/DNSCrypt (pré-requisito para ECH funcionar);
    
*   Validar que ECH está ativo no browser (about:config, flags, testes online);
    
*   Desabilitar WebRTC. Não é SNI diretamente, mas WebRTC vaza IP real mesmo com VPN/ECH, o que permite correlação e é um complemento lógico;
    

O ECH é a peça que conecta a série de proteções que temos construído aqui no blog. DNSCrypt protege a resolução de nomes, HTTPS protege o conteúdo e o ECH protege o handshake.

Sem ECH, o SNI é o último canal de metadados aberto para observadores de rede. Com ECH, esse canal fecha. O ISP deixa de saber qual site você visita, não apenas o que você faz dentro dele.

A adoção ainda está em progresso. Mas o suporte nos navegadores já é universal, o Cloudflare empurra a adoção no lado servidor, e distribuições Linux como Ubuntu e Fedora facilitam a configuração de DoH/DoT como requisito base.

Se você já configurou DNSCrypt seguindo a série anterior, validar que o ECH está habilitado no navegador é questão de segundos.

Referências e leitura complementar:

*   RFC 8744 - Issues and Requirements for SNI Encryption in TLS
    
*   IETF Draft ECH: https://datatracker.ietf.org/doc/draft-ietf-tls-esni/
    
*   Cloudflare ECH: https://blog.cloudflare.com/announcing-encrypted-client-hello/
    
*   CDT - Encrypted Client Hello: Closing the SNI Metadata Gap
    
*   Cisco ECH Defense Strategies (análise do lado corporativo/firewall)
    
*   https://defo.ie/ech-check.php (verificador ECH)
    
*   https://crypto.cloudflare.com/cdn-cgi/trace (verificador Cloudflare)
    
*   Série DNSCrypt: https://esli.blog.br/dnscrypt-proxy
    
*   Não confie no seu provedor: https://esli.blog.br/nao-confie-no-seu-provedor-de-internet
    
*   Qual melhor VPN: https://esli.blog.br/qual-melhor-vpn
    
*   Qual melhor navegador: https://esli.blog.br/qual-melhor-navegador