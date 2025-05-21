---
title: "Certificados e OpenSSL"
datePublished: Mon Nov 18 2019 19:20:45 GMT+0000 (Coordinated Universal Time)
cuid: ckqitqq7302w3t6s18ocn4id5
slug: certificados-e-openssl
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1625017967681/K74ukK6Y1.jpeg
tags: ssl, linux, https, security, tls

---

Uma breve explicação sobre cada um deles e como se usa ;-)

Atualmente o Google e seu mecanismo de busca estão punindo quem não utiliza protocolos de segurança, antes marcando os sites seguros, hoje marcando sites como inseguros e rebaixando-os dos resultados quando um usuário realiza alguma pesquisa, no futuro breve, os navegadores irão exibir uma página de alerta ao usuário, o que certamente causará o abandono do site pelo usuário leigo.

Toda vez que você entra em um site e realiza algum tipo de login, está dizendo, “sim, eu confio neste site, por isso estou disposto a compartilhar minhas informações pessoais com ele”. Esses dados podem incluir seu nome, sexo, endereço físico, endereço de e-mail, geolocalização, dados e atividades feitas nas redes sociais, cartão de crédito, carteiras digitais, dentre outras…

Mas como você sabe que pode confiar em um site específico? Em outras palavras, o que o site está fazendo para proteger sua transação, para que você possa confiar nela?

Abaixo, um report de um dos sites que administro que ainda não tem o HSTS (leia mais abaixo), 1/3 do tráfego diário ainda é inseguro.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017961045/pYaVImMOx.png)

## HTTPS

Por padrão, um site não é seguro se ele usa o protocolo HTTP. 
Adicionar um certificado configurado através do host do site à rota pode transformar o site de um site HTTP não seguro em um site HTTPS seguro. 
O ícone de cadeado geralmente indica que o site está protegido por HTTPS.

HTTP é uma sigla em inglês para Hypertext Transfer Protocol ou “protocolo de transferência de hipertexto”. Trata-se, portanto, de um código responsável por fazer a comunicação entre os dados de uma página na internet e quem está fazendo o acesso, geralmente pela porta 80. Ou seja, é por causa dele que você está lendo este artigo.

O HTTP Over SSL ou HTTPS é uma implementação idêntica do HTTP mas com uma camada adicional, o TLS.

Já ví artigos afirmarem que HTTPS, o S é de Secure (Seguro), mas isto induz ao erro, pois existe o **SHTTP** que é descrito na RFC: h[ttps://tools.ietf.org/html/rfc2660](https://tools.ietf.org/html/rfc2660), este sim é o “Secure Hypertext Transfer Protocol”.

O **HTTP Over SSL** (**HTTPS**) é descrito na RFC 2818: [https://tools.ietf.org/html/rfc2818](https://tools.ietf.org/html/rfc2818)

Na documentação desta RFC há uma descrição excelente: “Conceitualmente, HTTP/TLS é muito simples. Basta usar HTTP sobre TLS exatamente como você usaria HTTP sobre TCP”

## SSL e TLS

TLS é a geração atual do antigo protocolo SSL (Secure Socket Layer).

O TLS atualmente na versão 1.2 (a mais adotada) está descrito na RFC [https://www.ietf.org/rfc/rfc5246.txt](https://www.ietf.org/rfc/rfc5246.txt)

Há o TLS 1.3 em [https://www.ietf.org/rfc/rfc8446.txt](https://www.ietf.org/rfc/rfc8446.txt)

A função do HTTPS é exatamente a mesma do HTTP, porém o protocolo apresenta uma camada que criptografa os dados e a comunicação entre o servidor e quem está acessando, o SSL e geralmente é realizada na porta 443. Desse modo, o servidor que é acessado para repassar os dados das páginas para o seu computador precisa ser autenticado por meio de chaves e certificados digitais para que seja garantida a segurança na conexão à página.

O agente que atua como cliente HTTP também atua como cliente TLS. Ele deve iniciar uma conexão com o servidor na porta apropriada e, em seguida, envia um “Olá” (é o TLS ClientHello) para iniciar o “aperto de mão” (é o TLS handshake) (assim ambos se conhecem e se comunicam ba-du-tsss rsrsss) Quando o handshake termina, o cliente inicia a primeira solicitação HTTP. Todos os dados HTTP seguintes SERÃO enviados com o tipo TLS “dados de aplicação”, daí o HTTP sobre TLS.

## CA

Você que está lendo este artigo confia no certificado que está neste site. O seu Sistema Operacional e/ou seu navegador possui instalado a CA (Autoridade de Certificação) e seu navegador confia nos certificados emitidos por ela.

E através de algumas validações, meu site possui um certificado válido emitido pela CA que seu computador confia.

Uma destas validações é o OCSP (Protocolo de Certificação Online de Estado) descrito na RFC 6960: [https://www.ietf.org/rfc/rfc6960.txt](https://www.ietf.org/rfc/rfc6960.txt)

Outras validações, como invalidação de certificados e autoridades são enviadas ao cliente no formato de atualização (este é um dos motivos da Microsoft ainda enviar atualizações ao Windows Vista e 7 e há bem pouco tempo atrás ao Windows XP!)

No Linux a lista de CA raiz estão em:

`/usr/share/ca-certificates/`
`/usr/local/share/ca-certificates/`
`/etc/ssl/certs/`


Já acessou algum site do Governo Brasileiro e teve erro de TLS/SSL? Pois então, os certificados são gerados pelo órgão criado em 2001 voltado para Infraestrutura de Chaves Públicas Brasileira — ICP-Brasil, porém, a CA raiz desta ‘autoridade’ não está nos sistemas ou navegadores, ou seja, o cliente não confia! O usuário deve baixar a CA raiz e instalar ele próprio em seu S.O. e/ou navegador para acessar sem erros.

Desde 2008 o ITI (Instituto Nacional de Tecnologia da Informação Brasileiro [https://www.iti.gov.br/](https://www.iti.gov.br/)) tenta incluir a AC raiz brasileira na lista de certificados confiáveis do Firefox, a entidade brasileira ainda carece de uma auditoria exigida pela Mozilla, o que justifica a não inclusão do certificado no navegador. Não consegui dados sobre se existe um processo para inseri-los no Google Chrome.

## HSTS

Diversos sites para implementar o HTTPS acabam utilizando o redirect nos servidores, isto era correto até 2012, de lá para cá existe o protocolo HSTS, descrito na RFC 6797: [https://tools.ietf.org/html/rfc6797](https://tools.ietf.org/html/rfc6797)

Este protocolo facilmente implementado com 1 linha apenas no Apache, Caddy e Nginx

O HTTP Strict Transport Security (HSTS) é um mecanismo de política de segurança que ajuda a proteger sites contra ataques de downgrade de protocolo e seqüestro de cookies. A ativação do HSTS declara que os navegadores da Web devem interagir apenas com o servidor usando uma conexão HTTPS segura e nunca por meio de uma conexão HTTP insegura.

A política especifica um período de tempo durante o qual o servidor deve ser acessado de forma segura.

Ao usar o HSTS, o cliente entende que 100% da comunicação com aquele domínio DEVE ser via HTTPS, e o cache no header informa quando ele deve verificar novamente (ou seja, mesmo desativando o HSTS, o cliente continuará a forçar e usar o HTTPS por 1 ano — tempo médio usado na configuração do cache).
Ou seja, com o HSTS, para alguém conseguir algum ataque como envenenar cookies ou outro tipo de man-in-the-middle, precisaria não somente atacar o servidor do site original e desabilita-lo, mas também esperar 1 ano para poder enviar um http para o cliente alvo.

Para validar o HSTS em um site, basta executar o seguinte comando no terminal:

```
$ curl -s -D- [https://esli-nux.com/](https://esli-nux.com/) | grep -i Strict
strict-transport-security: max-age=31536000; includeSubDomains; preload
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017962719/yfQF_FGGq.png)


**Habilitar o HSTS no Apache, Nginx e Caddy:**

No Apache

```
<IfModule mod_headers.c>
# this domain should only be contacted in HTTPS for the next 6 months
Header add Strict-Transport-Security “max-age=15768000”
</IfModule>
```

No Nginx

Adicione o seguinte parâmetro no arquivo de configuração do Nginx:

```add_header Strict-Transport-Security max-age=15768000;```

No Caddy:

Adicione no Caddyfile:

```header / Strict-Transport-Security “max-age=31536000;”```


Para verificar toda a comunicação pode-se executar no terminal:

`$ openssl s_client -showcerts -connect esli-nux.com:443`

Será obtido a lista de certificados, o detalhamento do handshake (cifra, protocolo, sessão…)



## Certificados Auto Assinados

Uma CA é uma organização confiável que pode emitir um certificado digital.

TLS e SSL podem tornar uma conexão segura, mas o mecanismo de criptografia precisa de uma maneira de validá-la; este é o certificado SSL / TLS.

O TLS usa um mecanismo chamado criptografia assimétrica, que é um par de chaves de segurança chamadas chave privada e chave pública.

Um certificado auto assinado é um certificado TLS / SSL assinado pela pessoa que o cria e não por uma CA confiável. É fácil gerar um certificado autoassinado a partir de um computador e pode permitir que você teste um site seguro sem comprar um certificado assinado pela CA.

Embora o certificado auto assinado seja definitivamente arriscado para uso em produção, é uma opção fácil e flexível para o desenvolvimento e teste nos estágios de pré-produção. Mas não é difícil acha-lo em uso em ambientes restritos, como redes internas.

Várias ferramentas de código aberto estão disponíveis para gerenciar certificados TLS / SSL.

O mais conhecido é o **OpenSSL**, que está incluído em quase todas as distribuições Linux. No entanto, outras ferramentas de código aberto também estão disponíveis.

Alguns outros:

**EasyRSA** — Utilitário de linha de comando para criar e gerenciar uma CA PKI

**CFSSL** — utilitário de PKI/TLS da Cloudflare

**Lêmure** — Ferramenta de criação de TLS da Netflix

## Como criar um certificado OpenSSL

Podemos criar certificados por conta própria. Este exemplo gera um certificado autoassinado usando o OpenSSL.

Crie uma chave privada usando o comando **openssl**:

`$ openssl genrsa -out chaveprivada.key 2048`

Crie uma solicitação de assinatura de certificado (CSR) usando a chave privada gerada na etapa anterior (irá responder dados de localização, email e empresa):
`$ openssl req -new -key chaveprivada.key -out solicitacaochave.csr`

Crie um certificado usando seu CSR e chave privada:

`$ openssl x509 -req -days 365 -in solicitacaochave.csr -signkey chaveprivada.key -out certificado.crt`

## converter certificado pfx para pem e usar no cUrl

Não importa como está o certificado ele pode ser convertido para outros formatos usando o OpenSSL.

No exemplo abaixo mostro como usar um certificado SSL junto com o cUrl para login em site via linha de comando.

Um exemplo pratico disto é usar o curl para validar o login com certificado digital no site da sefaz.

Você possui o certificado digital (arquivo .cer), e a chave privada no formato de arquivo .pfx (Personal Information Exchange file).

Para conseguir será necessário converter o arquivo para PEM (X.509), no passo abaixo será necessário inserir a passphrase e a senha:

`$ openssl pkcs12 -in original.pfx -out convertido.pem`

Agora é necessário dividir o arquivo PEM, ou seja, separar o PEM e gerar arquivos individuais para a chave privada, certificado e CA dele:

Gerar CA root do pfx:

`$ openssl pkcs12 -in original.pfx -out ca.pem -cacerts -nokeys`

Gerar a chave privada do cliente através do pfx:

`$ openssl pkcs12 -in original.pfx -out client.pem -clcerts -nokeys`

Gerar a chave a partir do .pfx:

`$ openssl pkcs12 -in original.pfx -out key.pem -nocerts`

Agora é possível fazer a autenticação via cUrl no site da sefaz usando o pfx convertido:

`$ curl -k https://www.site.exemplo.com -v --key key.pem --cacert ca.pem --cert client.pem`

## Encriptar e Descriptar um arquivo usando o OpenSSL

Outro caso de uso do OpenSSL é para trocar arquivos encriptados.

Em primeiro lugar, a pessoa que irá receber o arquivo precisa me enviar sua chave publica.

Para gerar uma chave ela deve seguir os passos a seguir:

Gerar uma chave PEM privada a partir de uma id_rsa já existente:

`$ openssl rsa -in ~/.ssh/id_rsa -outform pem > transfer.pem`

Gerar uma chave PEM publica a partir de uma id_rsa já existente:

`$ openssl rsa -in ~/.ssh/id_rsa -pubout -outform pem > transfer.pub.pem`

Pronto, foi gerado uma chave chamada “transfer”, nela, há a privada e a publica, a pessoa deve me enviar a chave pública somente.

Agora, irei gerar uma chave randômica de 32 byte (256 bit):

`$ openssl rand -base64 32 > chave.bin`

Encriptando a chave usando minha random e a publica que recebi:

`$ openssl rsautl -encrypt -inkey transfer.pub.pem -pubin -in chave.bin -out chave.bin.enc`

Por fim, vou encriptar meu arquivo:

`$ openssl enc -aes-256-cbc -salt -in backup.zip -out backup.enc -pass file:./chave.bin`

Para a pessoa que irá receber, devo enviar somente os arquivos .enc (o arquivo encriptado e a chave randômica gerada e encriptada).

Primeiro, irá descriptografar a chave que foi enviada, para isto, o receptor do arquivo vai usar sua própria chave privada (transfer.pem) que ele gerou e enviou a pública para que eu encriptasse o arquivo:

`$ openssl rsautl -decrypt -inkey id_rsa.pem -in chave.bin.enc -out key.bin`

Com o passo acima, o arquivo key.bin da pessoa e o meu chave.bin terão o mesmo conteúdo pois foi usado a chave publica para encriptar (eu) e a chave privada para descriptar (o receptor do arquivo).

Finalmente, descriptografar o arquivo:

`$ openssl enc -d -aes-256-cbc -in backup.enc -out backup.zip -pass file:./chave.bin`

## OpenPGP, PGP, and GPG

Todos são padrões de criptografia, funcionam através de chaves assimétricas, cada usuário gera em seu computador um par de chaves: uma pública e uma secreta. A pública é distribuída e permite que qualquer um criptografe dados de modo que só quem possui a chave secreta correspondente possa descriptografar.

PGP, ou Pretty Good Privacy é proprietário, atualmente pertence a Symantec.
OpenPGP é do mesmo criador do PGP, mas aqui é uma versão Open Source para uso público.

GPG ou GnuGPG significa “GNU Privacy Guard” é amplamente usado nos sistemas Linux.

Da mesma maneira que usamos o openssl para transferir o arquivo acima descrito, podemos usar o OpenPGP, PGP ou GPG para “assinar” ou encriptar também.

Uma das maneiras mais simples de uso é através do gpg assinar seus commits e tags num repositório do Git.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017964302/Rdn6b9QBN.png)

Dois commits: Um assinado e outro não, na interface do Github.

Ou seja, além de usar um protocolo de criptografia de chave publica para ‘fechar’ a comunicação entre meu repositório local e o servidor Git (SSH) ou até mesmo usando o HTTPS, eu gerei uma chave gpg e assino todos meus commits (a chave publica está no servidor e durante o commit forneço minha senha).

Garanto portanto que eu estou me comunicando com o servidor correto (SSH) e o servidor garante que o commit foi realizado por mim.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1625017966196/xRrwP97_5.png)

Outro formato de uso das assinaturas é no envio de e-mail. Você pode trocar chaves entre as pessoas que irão enviar e receber emails e eles serão encriptados, qualquer interferência no email não conseguirá obter a mensagem.
Há 2 extensões que testei que facilitam o processo de criar as chaves e incorporam no seu cliente de email:

Webmail (extensões no Chrome e Firefox):
[FlowCrypt](https://flowcrypt.com/): somente para o Gmail (achei o melhor!) atualmente utilizo ele, nao há restrições para conta gratuita [https://flowcrypt.com/me/esli](https://flowcrypt.com/me/esli)

[Mailvelope](https://www.mailvelope.com/): Gmail e outros (Outlook, Yahoo)

Thunderbird: [Enigmail](https://enigmail.net/index.php/en/)

Android: Alguns Apps como o “[OpenKeychain: Easy PGP](https://play.google.com/store/apps/details?id=org.sufficientlysecure.keychain)” permitem integrar com alguns clients de mensagem e email, como o K-9 Mail por exemplo.

O único provedor de email que conheço e fornece suporte ao PGP é o ProtonMail (que cria contas @prontonmail.com e @pm.me).

Bem, é isto ;-)
Espero ter sanado algumas dúvidas ou criado novas… 
Qualquer coisa, pinga no meu blog: [https://esli-nux.com](https://esli-nux.com)