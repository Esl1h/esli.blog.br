---
title: "Yubikey #3 - 2FA console terminal, ssh e sudo"
datePublished: Tue Sep 28 2021 14:25:19 GMT+0000 (Coordinated Universal Time)
cuid: cku4693v9062grps1e4gqa86k
slug: yubikey-console-sudo-ssh
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1632780100244/GfMkLVNcT.jpeg
tags: authentication, linux, security, console, ssh

---

Como visto anteriormente, o uso da chave é bem simples.

Não necessita usar os apps (caso já utilize algum outro gerador de token via mobile, por exemplo).

Cada site/serviço/app/ferramenta possui seu caminho para poder habilitar a chave, geralmente nas configurações de segurança, próximo onde faz a alteração da senha.

Os serviços que não suportam a chave física, suportam o 2FA via app.

Mas o ponto legal é que existe uma base de conhecimento no site da Yubico com o passo-a-passo para configurar sua chave em diversos sites/serviços.

## Como configurar sua YubiKey

Configurar seu YubiKey não é tão diferente de configurar autenticação de dois fatores baseada em aplicativo. Se você estiver usando um YubiKey (não outro autenticador de hardware), faça o seguinte:

Conecte sua YubiKey.

Vá para Yubico.com/setup e clique no dispositivo.

Navegue pela lista de aplicativos compatíveis e encontre o que deseja proteger.

Siga as instruções.

No meu caso, que possuo uma Yubikey 5 NFC, o site é https://www.yubico.com/br/setup/yubikey-5-series/ na opção Compatibles Services.

## Yubico API

Para ter o ID e Secret da API, insira um email e toque na chave para preencher os seguintes campos:

https://upgrade.yubico.com/getapikey/

Apesar do nome ser 'upgrade', ter a API é grátis!

Exemplo:

```plaintext
Client ID:	58107
Secret key:	8xpmpAy2OtmZAAeXP/0NUed/aLB=
```

## Configuração inicial - libpam-yubico

Há duas formas de configurar, uma para usar o libpam-yubico e a outra usa o libpam-u2f.

Você pode configurar os dois modos no mesmo host, mas para cada arquivo no pam.d deverá usar apenas uma das formas (não faz sentido usar 2 tokens no mesmo login)

Crie o arquivo de autenticação em /etc/yubikeys\_mappings .

Um adicional é possuir na home do seu usuário os dados da sua chave, caso nos arquivos em pam.d/ seja omitido o arquivo de chaves, ele irá buscar na home do user: `.yubico/authorized_yubikeys`

Neste arquivo (o principal em /etc/), você apontará cada login (username) com a sua chave (apenas os 12 primeiros caracteres, pois eles identificam o hardware). E opcionalmente, pode manter um arquivo individual na home de cada um.

O template é o seguinte:

```plaintext
#username:12charsdakey
esl1h:ccccedrrdvhv
```

Se o user tiver mais do que uma chave, separe os 12 primeiros caracteres de cada um usando ponto-e-virgula. Uma linha por usuário.

Adicione o ID e a Secret na linha do dpkg, ao rodar o comando:

```plaintext
sudo dpkg-reconfigure libpam-yubico
```

Na tela seguinte, Perfis PAM para habilitar (PAM enable profile), mantenha como está.

## Configuração inicial - libpam-u2f

Instalando a lib:

```plaintext
sudo apt install libpam-u2f pamu2fcfg
```

Criar na home do user o path para as configs:

```plaintext
mkdir -p ~/.config/Yubico
```

Criando o arquivo de configuração da autenticação:

```plaintext
pamu2fcfg > ~/.config/Yubico/u2f_keys
```

Se você cadastrou um PIN na chave, será solicitado. Após, toque na chave e ele vai popular o arquivo...

## Padronização

Para ambas configurações acima:

No arquivo /etc/pam.d/common-auth adicione a opção try\_first\_pass como parâmetro no pam\_unix.so. Como no exemplo:

```plaintext
auth [success=1 default=ignore]  pam_unix.so nullok_secure try_first_pass
```

## Yubikey para logar no terminal

Adicione no inicio do /etc/pam.d/login:

```plaintext
auth sufficient pam_yubico.so id=58107
# troque para o seu ID!
```

Não vai modificar o login da interface gráfica, apenas o login no console do terminal!

Para usar o libpam-u2f, ao invés da linha acima, coloque a seguinte:

```plaintext
auth required pam_u2f.so
```

A diferença entre eles é o retorno visual, enquanto o libpam-yubico irá apresentar o prompt solicitando a key para o usuário, com o u2f nada visual será apresentado, apenas na chave (meu modelo é a 5 NFC) o botão ficará piscando, indicando para adicionar o token (tocar na chave).

## 2FA no sudo

Vamos usar o libpam-u2f.

Com o u2f configurado, basta configurar a autenticação, neste caso o arquivo /etc/pam.d/sudo para solicitar a chave.

Logo abaixo da linha com `@include common-auth`, adicione:

```plaintext
auth required pam_u2f.so
```

Pronto!

Ao rodar o comando sudo, primeiro adicione a senha + \[enter\] e em seguida, toque na chave para inserir o token.

## SSH

Outra opção é adicionar a linha acima no /etc/pam.d/common-auth, forçando o uso do token em todas as autenticações que usam o PAM.

Usando o u2f e com os passos de configuração já aplicados, basta inserir no `/etc/pam.d/sshd` abaixo da linha `@include common-account`:

```plaintext
auth       required   pam_u2f.so
```

PORÉM só vai funcionar em localhost!

Usando um ambiente com host remoto + yubikey para este servidor com a configuração acima, não irá funcionar. É necessário algumas configurações e parâmetros para o sshd\_config com o objetivo de obter um ambiente com SSH e 2FA.

Outras alternativas são: usar o yubikey com o FIDO U2F apenas para gerar a chave ssh, usar somente o token yubikey para logar via SSH (sem senha, chave ou 2FA) através do PGP ou PIV (próximos artigos!)