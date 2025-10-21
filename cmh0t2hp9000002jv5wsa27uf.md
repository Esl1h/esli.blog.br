---
title: "Criando uma extensão de navegador"
datePublished: Tue Oct 21 2025 16:55:34 GMT+0000 (Coordinated Universal Time)
cuid: cmh0t2hp9000002jv5wsa27uf
slug: criando-uma-extensao-de-navegador
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1761065360705/8e5ce357-24c6-4411-923b-27ab3ad83899.webp
tags: css, js, javascript, web-development, chrome, html, plugins, developer, frontend-development, chrome-extension, brave, brave-browser, desenvolvimento-web

---

Uma extensão que permite iniciar rapidamente os sites do Proton a partir de uma grade personalizável, com status de conexão e atalhos.  

![Captura de tela da extensão Proton Launcher](https://github.com/Esl1h/proton-launcher-extension/raw/main/docs/screenshot.png align="center")

Primeiro de tudo, não sou afiliado à Proton AG. Mas se quiserem me contratar, é só ligar.

Esta extensão é um projeto independente, sem vínculos com a Proton AG ou seus produtos oficiais. Esta extensão é para acessar serviços Proton, apenas um pop-up com links para serviços Proton.

Observação: criei isso para ser usado como um modelo para qualquer pessoa criar uma extensão personalizada para acessar qualquer outro serviço web. Basta editar o config.json e colocar algumas imagens.

Por quê? Então, por que não! Mas não era só para salvar nos favoritos? Sim. No entanto, eu queria criá-lo.

Sim, coloquei capturas de tela do Brave Browser na Google Chrome Web Store ;-)

![Captura de tela da extensão Proton Launcher](https://github.com/Esl1h/proton-launcher-extension/raw/main/docs/on-brave.png align="left")

## **Instalação**

Só para usar:  
encontre na Chrome Web Store: [**https://chromewebstore.google.com/detail/proton-launcher-unofficia/ajniphjinhppblnkeolojgopecjlnecn**](https://chromewebstore.google.com/detail/proton-launcher-unofficia/ajniphjinhppblnkeolojgopecjlnecn)

Para desenvolvedores:  
Baixe ou clone este repositório: [**https://github.com/Esl1h/proton-launcher-extension**](https://github.com/Esl1h/proton-launcher-extension)

No seu Chrome/Brave, vá para **chrome://extensions** e habilite o modo desenvolvedor.

Clique em “Carregar descompactado” e selecione a pasta do projeto git.

```bash
project/ 
├── manifest.json # extension manifest file 
│
├── background # service worker for events and integrations 
│ └── background.js 
│
├── config # configuration files 
│ ├── apps.json 
│ └── settings.json 
│
├── popup # launcher interface 
│ ├── popup.css 
│ ├── popup.html 
│ └── popup.js 
│
├── assets 
  ├── icons 
  │ ├── icon128.png 
  │ ├── icon16.png 
  │ └── icon48.png 
  └── proton-logo.svg
```

Edite os arquivos em `config/` para personalizar os aplicativos e preferências padrão. As configurações do usuário são salvas automaticamente.

Atalhos  
`Ctrl+Shift+L` – Abra rapidamente o lançador

A arquitetura segue um padrão MVC simplificado, onde popup.html serve como interface visual, popup.js atua como controlador gerenciando lógica e estado de interação e background.js funciona como service worker para operações em segundo plano.

  
A estrutura de dados é minimalista: cada favorito é armazenado como um objeto, tudo mantido em uma matriz sob a chave de favoritos.

  
Manifest V3 define permissões (nenhuma neste meu caso!) e declara uma CSP (Política de Segurança de Conteúdo) restritiva, garantindo proteção contra XSS.

O manifest.json define permissões, ícones, scripts e o pop-up (**popup.html + popup.js**) exibido quando o usuário interage com o ícone de extensão. Resumindo: um manifest.json (modelo) obrigatório, o pop-up (CSS + HTML) e o núcleo — JavaScript simples (sem frameworks) que manipula o carregamento de links/ícones, validação de URL e renderização.

Há também um script em segundo plano (background.js) que funciona como um trabalhador persistente, mas executa uma inicialização limpa — a ideia é mantê-lo para uso futuro. A interface pop-up lê os aplicativos configurados (config/apps.json), os exibe em uma grade clicável e abre o link em uma nova aba (minha intenção era — ou é? — abrir o webapp/PWA).

**aplicativos.json:** Cada ícone tem um título, ícone, URL, posição e descrição. Optei por ocultar a descrição na exibição, mas a mantive no JSON.

**configurações.json:** Tamanho da grade, intervalo de atualização e um único tema chamado gradiente.

---

  
É isso. Não há muito a dizer sobre isso... foi um dos meus pequenos projetos de fim de semana.

Ele foi criado para ser simples, rápido, seguro e fácil... tanto para alterar quanto para usar como modelo inicial para criar algo maior.

Não está completo, a ideia (há um ícone de configuração na interface pop-up) seria uma página que você pode preencher com os links desejados (e opcionalmente um ícone se não quiser o favicon ou o padrão de alguma biblioteca de ícones js).

Packing (crx) e envio para Web Store

Ah, antes de terminar, para enviar a extensão para a Chrome Web Store, você precisa acessar o painel do desenvolvedor (custa US$ 5 para ativar) em [**https://chrome.google.com/webstore/devconsole/**](https://chrome.google.com/webstore/devconsole/).

Crie um novo item, preencha todos os campos para requisitos, descrição e informações e carregue o arquivo .zip.

Para criar o arquivo .crx, vá diretamente ao navegador, abra o gerenciador de extensões e selecione a opção “pack extension” (após ativar o modo de desenvolvedor, é claro).

%[https://esli.blog.br/how-to-create-a-custom-web-browser-extension]