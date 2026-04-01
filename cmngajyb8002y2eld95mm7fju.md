---
title: "DNS criptografado no Android: InviZible Pro e alternativas"
datePublished: 2026-04-01T16:59:49.753Z
cuid: cmngajyb8002y2eld95mm7fju
slug: dns-criptografado-no-android-invizible-pro-e-alternativas
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/13978cc0-82c3-4187-99b8-86054a5bbcd4.jpg
tags: dns, https, security, privacy, android, tls, dns-over-https, dns-over-tls, dns-over-quic, invizible

---

Terceiro e último artigo da série sobre DNS criptografado. No [primeiro](https://esli.blog.br/dnscrypt-dns-stamps-e-dns-criptografado-o-guia-que-faltava), expliquei o protocolo DNSCrypt, os DNS Stamps, relays e o ecossistema de DNS criptografado. No [segundo](https://esli.blog.br/dnscrypt-proxy-no-linux-configurando-dns-criptografado), configurei o dnscrypt-proxy no Linux com NextDNS.

Agora é a vez do Android. O objetivo é o mesmo: queries DNS criptografadas via DoH usando o NextDNS como resolver, mas com o dnscrypt-proxy rodando via InviZible Pro, alternativas e o que o Android oferece nativamente.

## O que o Android já oferece (e por que não é suficiente)

Desde o Android 9, existe a opção "DNS Privado" em Configurações > Rede. Funciona com DoT (DNS-over-TLS) apontando para um hostname do tipo `abc123.dns.nextdns.io` (onde `abc123` é seu config ID do NextDNS).

Funciona? Sim. Mas tem limitações:

*   Só suporta DoT. Sem DoH, sem DNSCrypt, sem ODoH.
    
*   Sem cache local configurável.
    
*   Sem controle sobre qual app usa qual DNS.
    
*   Sem firewall integrado.
    
*   Sem fallback configurável.
    
*   Se o servidor DoT não responder, o Android pode fazer fallback para o DNS do provedor sem avisar (dependendo da opção: "automático" faz fallback, "estrito" bloqueia).
    
*   Não há logs para debug.
    

Para a maioria das pessoas, DNS Privado com NextDNS no modo estrito já é um avanço enorme sobre o DNS do provedor. Mas se você quer controle real, precisa de algo mais.

## InviZible Pro: a solução completa

O InviZible Pro é um aplicativo Android open-source que integra três módulos de privacidade em um único pacote:

| Módulo | Software subjacente | Função |
| --- | --- | --- |
| DNSCrypt | dnscrypt-proxy | DNS criptografado |
| Tor | tor | Anonimização de tráfego |
| I2P | Purple I2P (i2pd) | Acesso à rede I2P |

Disponível no F-Droid (versão sem restrições) e na Google Play. O código-fonte está em [https://github.com/Gedsh/InviZible](https://github.com/Gedsh/InviZible) sob licença GPLv3.

### O que ele faz

**Funciona com e sem root.** Com root, redireciona tráfego via iptables (mais eficiente, sem overhead de VPN). Sem root, cria uma VPN local (o tráfego não sai do dispositivo, apenas é redirecionado internamente para os módulos).

**Firewall integrado.** Controle granular por aplicativo: quem pode usar internet, quem passa pelo Tor, quem usa DNS direto. Isso substitui apps de firewall separados como o NetGuard.

**Kill switch.** Se o módulo cair, o tráfego é bloqueado. Sem vazamentos acidentais de queries em texto puro para o DNS do provedor.

**Bridges Tor.** Suporte a bridges obfs4, snowflake e webtunnel para contornar censura em redes restritivas.

**Configuração granular.** Acesso direto aos arquivos `dnscrypt-proxy.toml`, `torrc` e `i2pd.conf` dentro do app com editor de texto integrado.

**Logs em tempo real.** Cada módulo tem sua aba de log. Debug sem precisar de Termux ou logcat.

**Atualizações independentes.** Os binários do dnscrypt-proxy, Tor e Purple I2P são atualizados separadamente do app. Na versão mais recente (7.4.0, março de 2026): dnscrypt-proxy 2.1.15, Tor 4.8.22, Purple I2P 2.59.0.

**Stealth Mode.** Evasão de DPI (Deep Packet Inspection) para cenários de censura.

Em resumo: substitui VPN + firewall + adblock DNS em um único app, sem ads, sem rastreio, sem assinatura para as funcionalidades core. A versão paga destrava apenas o tema Material Design escuro.

## Configurando NextDNS no InviZible Pro

### Pré-requisitos

*   InviZible Pro instalado (F-Droid: `pan.alexander.tordnscrypt.stable` ou Play Store)
    
*   Conta no NextDNS com um perfil configurado
    
*   Seu config ID (visível em https://my.nextdns.io na aba Setup)
    

### Passo 1: Obter o stamp

**Forma rápida:** acesse https://my.nextdns.io, vá em Setup Guide > Routers ou Linux, seção DNSCrypt. O stamp já está pronto com seu config ID.

**Forma manual:** acesse [https://dnscrypt.info/stamps/](https://dnscrypt.info/stamps/) e preencha:

| Campo | Valor |
| --- | --- |
| Protocol | DNS-over-HTTPS |
| IP Address | (vazio) |
| Host name | dns.nextdns.io |
| Hashes | (vazio) |
| Path | /SEU\_ID (ex: /abc123) |
| No logs | marcado |
| No filter | desmarcado |
| DNSSEC | marcado |

O path DEVE começar com `/`. Se quiser identificar o dispositivo no painel do NextDNS, use `/abc123/android-pixel` ou similar.

Copie o stamp gerado. Algo como:

```plaintext
sdns://AgMAAAAAAAAAAAAADmRucy5uZXh0ZG5zLmlvBy9hYmMxMjM
```

### Passo 2: Configurar via interface do InviZible Pro

1.  Abra o InviZible Pro
    
2.  No card DNSCrypt, toque no ícone de engrenagem
    
3.  Acesse **DNSCrypt servers** e desmarque todos os servidores pré-configurados
    
4.  Acesse **Own servers / DNS Stamp** (o nome pode variar conforme a versão)
    
5.  Cole o stamp do passo anterior
    
6.  Dê um nome ao servidor (ex: `nextdns`)
    
7.  Salve
    

### Passo 2 (alternativo): Editar o dnscrypt-proxy.toml

Se você prefere controle total (e já sabe o que está fazendo):

1.  Menu > Common Settings > Open text editor
    
2.  Selecione `dnscrypt-proxy.toml`
    
3.  Edite:
    

```toml
# Forçar uso exclusivo do NextDNS
server_names = ['nextdns']

# Cache local (recomendado, especialmente no plano free)
cache = true
cache_size = 4096
cache_min_ttl = 2400
cache_max_ttl = 86400

# Servidor estático com seu stamp
[static]
  [static.'nextdns']
  stamp = 'sdns://AgMAAAAAAAAAAAAADmRucy5uZXh0ZG5zLmlvBy9hYmMxMjM'
```

4.  Salve e feche o editor
    

### Passo 3: Reiniciar e validar

1.  No card DNSCrypt, pare o módulo (Stop) e inicie novamente (Start)
    
2.  Verifique os logs do DNSCrypt. Deve aparecer o servidor `nextdns` como ativo com status OK
    
3.  No navegador, acesse https://test.nextdns.io
    

Se tudo estiver correto, a página retorna seu config ID e status "connected" ou "using NextDNS".

Se retornar "not using NextDNS":

*   Revise o stamp (o path tem a `/` inicial? O config ID está correto?)
    
*   Verifique se não há outro servidor marcado nas configurações do DNSCrypt
    
*   Nos logs, procure mensagens de erro relacionadas ao stamp
    

### Passo 4: Ajustes adicionais

**DNS e Tor juntos (cuidado):**

Se o módulo Tor estiver ativo, por padrão as queries DNS são roteadas pelo Tor antes de chegar ao NextDNS. Isso:

*   Esconde seu IP real do NextDNS (pode ser desejável)
    
*   Aumenta a latência consideravelmente (indesejável para uso diário)
    
*   Pode fazer o NextDNS não aplicar regras baseadas em localização
    

Para desabilitar: Common Settings > desmarque "Route DNS through Tor".

**Consumo de bateria:**

O DNSCrypt sozinho consome pouco. O Tor consome mais. Se usar apenas o módulo DNSCrypt (sem Tor e sem I2P), o impacto na bateria é mínimo. Segundo a documentação do InviZible, Tor + DNSCrypt 24/7 consome em torno de 8%.

Exclua o InviZible Pro da otimização de bateria do Android para evitar que o sistema mate o processo em background. Vá em Configurações > Bateria > Otimização de bateria > InviZible Pro > Não otimizar. Consulte https://dontkillmyapp.com para instruções específicas do seu fabricante (Samsung, Xiaomi/MIUI e Huawei são os piores nesse aspecto).

**Rate limit no plano free do NextDNS:**

O plano gratuito permite 300k queries/mês com filtragem ativa. Após o limite, o DNS continua resolvendo mas sem bloqueio e sem logs. O cache agressivo no `dnscrypt-proxy.toml` (`cache_min_ttl = 2400`) ajuda a reduzir o número de queries.

Monitore em https://my.nextdns.io > Analytics.

## Alternativas ao InviZible Pro para DNSCrypt no Android

O InviZible Pro não é a única opção e dependendo do que você precisa (ou até onde está disposto a ir), outras ferramentas podem atender.

### personalDNSfilter

Open-source, disponível no F-Droid. Funciona como proxy DNS local com suporte a blocklists. Não usa dnscrypt-proxy internamente, mas suporta DoH e DoT como upstream. Mais leve que o InviZible, sem os módulos Tor/I2P. Se você quer apenas DNS filtrado sem o pacote completo de privacidade, é uma opção.

[https://www.zenz-solutions.de/personaldnsfilter-wp/](https://www.zenz-solutions.de/personaldnsfilter-wp/)

### AdGuard (versão completa)

A versão completa do AdGuard (não confundir com o AdGuard DNS) funciona como VPN local no Android e intercepta DNS. Suporta DoH, DoT e DNSCrypt como upstream. Tem bloqueio de ads ao nível de rede (não só DNS), filtro HTTPS para bloquear ads em conexões criptografadas e proteção contra tracking. A versão completa é paga e não está na Play Store (baixar via site oficial).

[https://adguard.com/en/adguard-android/overview.html](https://adguard.com/en/adguard-android/overview.html)

### Rethink DNS + Firewall

Open-source, no F-Droid. Combina firewall por app, DNS criptografado (DoH, DoT), blocklists e logs detalhados. Interface moderna. Não usa dnscrypt-proxy, mas suporta configuração de servidores DNS customizados via DoH. Não suporta o protocolo DNSCrypt nem stamps diretamente, mas aceita endpoints DoH como `https://dns.nextdns.io/abc123`.

[https://rethinkdns.com/](https://rethinkdns.com/)

### DNS Privado nativo do Android

Como mencionado, funciona apenas com DoT. Hostname do NextDNS: `abc123.dns.nextdns.io`. Sem firewall, sem cache configurável, sem logs. Mas é zero configuração e funciona em qualquer Android 9+.

### Comparativo rápido

| Feature | InviZible Pro | personalDNSfilter | AdGuard | Rethink DNS | DNS Privado |
| --- | --- | --- | --- | --- | --- |
| Protocolo DNSCrypt | Sim | Não | Sim | Não | Não |
| DoH | Sim (via stamp) | Sim | Sim | Sim | Não |
| DoT | Não diretamente | Sim | Sim | Sim | Sim |
| DNS Stamps | Sim | Não | Sim | Não | Não |
| Tor integrado | Sim | Não | Não | Não | Não |
| I2P integrado | Sim | Não | Não | Não | Não |
| Firewall por app | Sim | Não | Sim | Sim | Não |
| Open-source | Sim (GPLv3) | Sim | Parcial | Sim | N/A |
| Root necessário | Não (VPN local) | Não | Não | Não | Não |
| F-Droid | Sim | Sim | Não | Sim | N/A |
| Custo | Grátis | Grátis | Pago | Grátis | Grátis |

## Resultado

Com o InviZible Pro configurado, seu Android passa a ter o mesmo nível de proteção DNS que o desktop com dnscrypt-proxy. Mesmo stamp, mesmo config ID do NextDNS, mesmas blocklists, analytics unificado no painel.

O setup completo da série: primeiro artigo para entender o ecossistema, segundo para configurar no Linux (Fedora e Arch), terceiro para Android. Três ambientes, um resolver, queries criptografadas em todos.

Se o seu provedor de internet pudesse cobrar por cada consulta DNS que bisbilhota, já teria financiado um programa espacial. Criptografe suas queries.

Outros artigos relacionados:

*   [DNSCrypt, DNS Stamps e DNS Criptografado: o guia que faltava](https://esli.blog.br/dnscrypt-dns-stamps-guia)
    
*   [dnscrypt-proxy no Linux: configurando DNS criptografado com NextDNS](https://esli.blog.br/dnscrypt-proxy-linux-nextdns)
    
*   [Privacidade e Segurança (2025)](https://esli.blog.br/privacidade-e-seguranca-2025)
    
*   [VPN não é o suficiente](https://esli.blog.br/vpn-nao-e-o-suficiente)
    
*   [Como fugir das propagandas na Internet](https://esli.blog.br/como-fugir-das-propaganda-na-internet)
    
*   [Qual melhor navegador](https://esli.blog.br/qual-melhor-navegador)
    

Referências:

*   https://github.com/Gedsh/InviZible
    
*   https://invizible.net/en/
    
*   https://f-droid.org/packages/pan.alexander.tordnscrypt.stable/
    
*   https://github.com/DNSCrypt/dnscrypt-proxy
    
*   https://dnscrypt.info/stamps/
    
*   https://my.nextdns.io
    
*   https://test.nextdns.io
    
*   https://dontkillmyapp.com
    
*   https://rethinkdns.com/
    
*   https://adguard.com/en/adguard-android/overview.html