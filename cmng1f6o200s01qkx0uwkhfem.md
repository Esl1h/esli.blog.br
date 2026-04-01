---
title: "dnscrypt-proxy no Linux: configurando DNS criptografado"
datePublished: 2026-04-01T12:44:10.757Z
cuid: cmng1f6o200s01qkx0uwkhfem
slug: dnscrypt-proxy-no-linux-configurando-dns-criptografado
cover: https://cdn.hashnode.com/uploads/covers/5df318ae066598ab275294bf/c4560645-055c-47ab-9803-e850d0d73984.jpg
tags: dns, https, security, privacy, tls, cryptography, dns-over-https, dns-over-tls, criptografia, dns-over-quic

---

Este é o segundo artigo da série sobre DNS criptografado. No [primeiro](https://esli.blog.br/dnscrypt-dns-stamps-e-dns-criptografado-o-guia-que-faltava), expliquei o que é o DNSCrypt, os protocolos de DNS criptografado, o conceito de DNS Stamps e relays. Se você caiu aqui direto, recomendo ler o anterior antes de prosseguir.

O objetivo agora é prático: instalar o dnscrypt-proxy no Linux e configurar o NextDNS como resolver exclusivo via DoH, usando o stamp gerado conforme descrito no artigo anterior.

Uso NextDNS em todos os meus dispositivos, conforme descrevi no [Privacidade e Segurança (2025)](https://esli.blog.br/privacidade-e-seguranca-2025). Aqui vou mostrar como configurar usando o dnscrypt-proxy ao invés do cliente oficial do NextDNS ou do DNS Privado nativo.

## Por que dnscrypt-proxy ao invés do cliente NextDNS?

O NextDNS oferece um CLI próprio (`nextdns` via `sh -c "$(curl -sL https://nextdns.io/install)"`). Funciona bem, é simples. Mas o dnscrypt-proxy oferece vantagens:

*   Suporta múltiplos protocolos (DNSCrypt, DoH, ODoH)
    
*   Permite trocar de provider sem mudar a stack
    
*   Suporte a Anonymized DNSCrypt e relays
    
*   Cache local com TTL configurável
    
*   Cloaking, blocklists locais, forwarding rules
    
*   Logs detalhados para debug
    
*   Funciona idêntico em qualquer distro
    

Se você quer apenas o NextDNS funcionando rápido, o CLI oficial resolve. Se quer controle granular ou planeja usar recursos além do que o NextDNS oferece nativamente, o dnscrypt-proxy é o caminho.

## Instalação

### Fedora

```bash
sudo dnf install dnscrypt-proxy
```

O pacote instala o binário, o serviço systemd e o arquivo de configuração em `/etc/dnscrypt-proxy/dnscrypt-proxy.toml`.

### Arch / EndeavourOS / Omarchy

```bash
sudo pacman -S dnscrypt-proxy
```

Mesma estrutura. Arch mantém o pacote atualizado com frequência.

### Debian / Ubuntu

```bash
sudo apt install dnscrypt-proxy
```

Atenção: as versões nos repositórios do Debian/Ubuntu podem estar desatualizadas. Verifique a versão com `dnscrypt-proxy --version` e compare com os releases em https://github.com/DNSCrypt/dnscrypt-proxy/releases. Se estiver muito defasado, instale manualmente via binário do GitHub.

## Obter o stamp do NextDNS

Duas formas:

**Forma 1 (recomendada):** acesse https://my.nextdns.io, vá em Setup Guide, selecione Linux ou Routers, e copie o stamp da seção DNSCrypt. Ele já vem com seu config ID embutido.

**Forma 2 (manual):** acesse https://dnscrypt.info/stamps/ e preencha:

| Campo | Valor |
| --- | --- |
| Protocol | DNS-over-HTTPS |
| IP Address | (vazio) |
| Host name | dns.nextdns.io |
| Path | /SEU\_ID (ex: /abc123) |
| No logs | marcado |
| DNSSEC | marcado |

Copie o stamp resultante (`sdns://...`).

Para identificar a máquina no painel do NextDNS, adicione o nome do dispositivo ao path: `/abc123/fedora-desktop`.

## Configuração

Edite o arquivo principal:

```bash
sudo vim /etc/dnscrypt-proxy/dnscrypt-proxy.toml
```

As configurações essenciais:

```toml
# Porta de escuta
listen_addresses = ['127.0.0.1:53', '[::1]:53']

# Forçar uso exclusivo do NextDNS
server_names = ['nextdns']

# Habilitar IPv6 se sua rede suportar
ipv6_servers = false

# Requerer DNSSEC, no-log e no-filter
require_dnssec = true
require_nofilter = false
require_nolog = true

# Cache local
cache = true
cache_size = 4096
cache_min_ttl = 2400
cache_max_ttl = 86400

# Desabilitar download automático de listas públicas
# (queremos APENAS o NextDNS)
# Comente ou remova as seções [sources] se existirem:
# [sources]
#   [sources.'public-resolvers']
#     ...
#   [sources.'relays']
#     ...

# Servidor estático com seu stamp
[static]
  [static.'nextdns']
  stamp = 'sdns://AgMAAAAAAAAAAAAADmRucy5uZXh0ZG5zLmlvBy9hYmMxMjM'
```

Substitua o stamp pelo gerado com seu ID real.

Sobre o cache: os valores acima são agressivos de propósito. `cache_min_ttl = 2400` (40 minutos) e `cache_max_ttl = 86400` (24 horas) reduzem significativamente o número de queries ao NextDNS. Se você está no plano free (300k queries/mês), isso faz diferença. Mesmo no plano pago, menos queries = menos latência para domínios frequentes.

## O inferno do systemd-resolved

Se você está no Fedora, Ubuntu ou qualquer distro que use systemd-resolved, prepare-se: ele escuta na porta 53 por padrão e vai conflitar com o dnscrypt-proxy.

Verifique antes de qualquer coisa:

```bash
ss -tlnp | grep ':53 '
```

Se aparecer `systemd-resolve` ouvindo na 53, você tem duas opções:

### Opção A: Desabilitar o systemd-resolved (recomendado)

Se você quer controle total do DNS e não precisa do resolved para nada:

```bash
sudo systemctl disable --now systemd-resolved
sudo rm /etc/resolv.conf
echo 'nameserver 127.0.0.1' | sudo tee /etc/resolv.conf
sudo chattr +i /etc/resolv.conf
```

O `chattr +i` torna o arquivo imutável. Sem isso, o NetworkManager vai sobrescrever o `resolv.conf` na primeira reconexão. Para editar futuramente: `sudo chattr -i /etc/resolv.conf`.

### Opção B: Convivência (dnscrypt-proxy em porta alternativa)

Se por algum motivo precisa manter o resolved (integração com VPN corporativa, mDNS/Avahi, etc.):

No `dnscrypt-proxy.toml`:

```toml
listen_addresses = ['127.0.0.1:5353']
```

No `/etc/systemd/resolved.conf`:

```ini
[Resolve]
DNS=127.0.0.1#5353
DNSStubListener=yes
```

```bash
sudo systemctl restart systemd-resolved
```

Funciona, mas adiciona uma camada desnecessária. O resolved vira um middleman que só repassa. A não ser que tenha um motivo concreto, vá com a Opção A.

### Nota para Arch com Hyprland / Window Managers

Se você não usa NetworkManager e configura rede via `iwd`, `dhcpcd` ou manualmente, o systemd-resolved provavelmente nem está ativo. Verifique com `systemctl status systemd-resolved`. Se estiver inativo, não precisa fazer nada. Apenas garanta que o `/etc/resolv.conf` aponte para `127.0.0.1`.

## Persistência do resolv.conf no Fedora com NetworkManager

O NetworkManager reescreve o `/etc/resolv.conf` a cada reconexão. Além do `chattr +i`, existe uma abordagem mais limpa: instruir o NetworkManager a não gerenciar DNS.

Crie o arquivo `/etc/NetworkManager/conf.d/no-dns.conf`:

```ini
[main]
dns=none
```

```bash
sudo systemctl restart NetworkManager
```

A partir daqui, o NetworkManager não toca no DNS. Seu `resolv.conf` com `nameserver 127.0.0.1` permanece intacto.

## Habilitando e iniciando

```bash
sudo systemctl enable --now dnscrypt-proxy.service
```

Verifique:

```bash
systemctl status dnscrypt-proxy
journalctl -u dnscrypt-proxy -f
```

Nos logs, procure linhas indicando que o servidor `nextdns` foi validado. Algo como:

```plaintext
[nextdns] OK (DoH) - rtt: XXms
```

Se aparecer erros de stamp ou servidor não encontrado, revise o stamp e verifique se não há servidores padrão habilitados competindo com a configuração estática.

## Validação

```bash
# Resolução via dnscrypt-proxy
dig @127.0.0.1 esli.blog

# Confirmar NextDNS ativo
curl -sL https://test.nextdns.io
```

O retorno do `test.nextdns.io` deve mostrar:

*   Status: Connected (ou Using NextDNS)
    
*   Protocol: DoH
    

Se retornar "not using NextDNS", revise:

1.  O stamp (path correto com `/SEU_ID`?)
    
2.  O `server_names` aponta para o nome correto?
    
3.  Algum servidor padrão ainda está habilitado nas sources?
    
4.  O `resolv.conf` aponta para `127.0.0.1`?
    

Teste também a filtragem. Acesse um domínio que deveria ser bloqueado pelas suas blocklists no NextDNS:

```bash
dig @127.0.0.1 [algum-site.bloqueado]
```

Se retornar `0.0.0.0` ou `NXDOMAIN`, a filtragem está funcionando.

## Configurações avançadas opcionais

### Anonymized DNS com relays

Se quiser ir além e usar relays para que o NextDNS não veja seu IP (via Anonymized DoH), adicione ao `dnscrypt-proxy.toml`:

```toml
[anonymized_dns]
routes = [
    { server_name='nextdns', via=['anon-relay-1', 'anon-relay-2'] }
]
```

Os relays disponíveis estão na lista de resolvers do dnscrypt-proxy. Isso adiciona latência, mas aumenta significativamente a privacidade. Se o NextDNS não deve saber de qual IP as queries vêm, essa é a forma.

### Blocklists locais

O dnscrypt-proxy suporta blocklists próprias, independentes do NextDNS. No `dnscrypt-proxy.toml`:

```toml
[blocked_names]
blocked_names_file = '/etc/dnscrypt-proxy/blocked-names.txt'
```

No arquivo `blocked-names.txt`, adicione domínios (um por linha, suporta wildcards):

```plaintext
*.facebook.com
*.tiktok.com
ads.*
```

Isso funciona mesmo se o NextDNS estiver fora do ar. É uma camada adicional de filtragem local.

### Query logging para debug

```toml
[query_log]
file = '/var/log/dnscrypt-proxy/query.log'
format = 'tsv'
```

Crie o diretório antes:

```bash
sudo mkdir -p /var/log/dnscrypt-proxy
sudo chown _dnscrypt-proxy:_dnscrypt-proxy /var/log/dnscrypt-proxy
```

O formato TSV facilita análise com `awk`, `cut` e companhia. Desligue em produção se privacidade for prioridade.

Meu dnscrypt-config.toml: https://github.com/Esl1h/dotfiles/blob/main/etc/dnscrypt-proxy/dnscrypt-proxy.toml

## Resultado

Mesmo stamp, mesma configuração do NextDNS, mesma proteção que você tem no Android (que será o tema do [próximo artigo](https://esli.blog.br/dnscrypt-android-invizible-pro-nextdns)). Suas queries DNS saem criptografadas, filtradas pelas mesmas blocklists, com analytics unificado no painel do NextDNS.

Seu provedor de internet que se contente em ver pacotes criptografados passando sem saber o que você está resolvendo.

* * *

Outros artigos relacionados:

*   [DNSCrypt, DNS Stamps e DNS Criptografado: o guia que faltava](https://esli.blog.br/dnscrypt-dns-stamps-guia)
    
*   [Privacidade e Segurança (2025)](https://esli.blog.br/privacidade-e-seguranca-2025)
    
*   [VPN não é o suficiente](https://esli.blog.br/vpn-nao-e-o-suficiente)
    
*   [Como fugir das propagandas na Internet](https://esli.blog.br/como-fugir-das-propaganda-na-internet)
    
*   [Usando o Brave, scriptlets, filter e wikiwand para melhor navegação](https://esli.blog.br/funcoes-avancadas-no-brave-browser)
    

Referências:

*   https://github.com/DNSCrypt/dnscrypt-proxy
    
*   https://github.com/DNSCrypt/dnscrypt-proxy/wiki/Configuration
    
*   https://dnscrypt.info/stamps/
    
*   https://my.nextdns.io
    
*   https://test.nextdns.io