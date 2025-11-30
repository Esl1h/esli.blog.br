---
title: "Melhor maneira de acessar a Wikipedia"
datePublished: Sun Nov 30 2025 18:09:27 GMT+0000 (Coordinated Universal Time)
cuid: cmim1bkka000002js6j2t4t5y
slug: melhor-maneira-de-acessar-a-wikipedia
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1764525747984/671e26f0-795e-456c-a6db-4afd51bd5f79.png
tags: css, javascript, wikipedia, brave, brave-browser, scriptlets, wikiwand

---

A Wikipedia, mesmo carregando conteúdo duvidoso e frequentemente tendencioso em certos temas, continua sendo um ponto de partida prático. Ela agrega referências, histórico de edições e um panorama razoável do assunto — útil para formar hipóteses, coletar termos-chave e seguir os links para fontes primárias. Em mecanismos de busca, é quase sempre a primeira resposta, não porque seja perfeita, mas porque cobre “quase tudo” com latência baixa e SEO agressivo. Use-a como índice, não como veredito.

O problema é encarar a interface: poluída, densa, com tipografia irregular e uma navegação que parece um teste de paciência de 2007. A hierarquia visual é inconsistente, os sumários colapsam quando querem e tabelas gigantes quebram o fluxo de leitura. Para ler sem sentir que abriu um prontuário em XML, existe o Wikiwand: um frontend alternativo que “embelezará” qualquer artigo da Wikipedia com tipografia decente, navegação lateral útil, melhor uso de espaço e atalhos. Você mantém o conteúdo e ganha uma experiência de leitura que não parece castigo — ideal para começar a pesquisa sem cansar os olhos antes da segunda referência.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1764524910416/7db728a0-e870-4119-9d4b-c26a3d9b677b.png align="center")

Como melhorar a interface? Stylesheets é a primeira resposta.  

* Stylus
    
* Stylebot
    
* User CSS (userContent.css / userChrome.css)
    
* Tampermonkey
    
* Violentmonkey
    
* Greasemonkey
    
* uBlock Origin (cosmetic filters)
    
* Dark Reader
    
* Stylrr
    
* CustomCSS for Fx
    
* Cascadea
    
* xStyle
    

Todos podem alterar o visual.

## Wikiwand

Uma excelente alternativa era o Wikiwand, com sua extensão [https://chromewebstore.google.com/detail/wikiwand-elevate-wikipedi/emffkefkbkpkgpdeeooapgaicgmcbolj](https://chromewebstore.google.com/detail/wikiwand-elevate-wikipedi/emffkefkbkpkgpdeeooapgaicgmcbolj)

O grande problema foi adicionarem AI e ads (muitas!) para forçar um plano de assinatura.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1764525046419/71d7f9ae-b25d-4acd-92bb-028d49c6f75d.png align="center")

Mesmo com o Brave, sem nenhuma configuração, fica poluído com os blocos:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1764525119484/8aec756b-33c4-4f26-8476-094cbcfae5f1.png align="center")

## Solução: Wikiwand + Brave scriptlets

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1764525252501/81a2c9e4-9fad-4cb6-a275-6d601316228a.png align="center")

Em custom filters, vamos adicionar:

```bash
! === Wikiwand ===
www.wikiwand.com##[class^="intro_footer__"], [class^="intro_header__"], [class^="intro_content__"], [class^="intro_wrapper__"]
www.wikiwand.com##[class^="extensions_wrapper__"]
www.wikiwand.com##[class*="wrapper_wrapper__"]:is([class*="wrapper_side__"], [class*="wrapper_atf__"], [class*="wrapper_incontent__"], [class*="wrapper_ob__"])
www.wikiwand.com##[class*="minimized_wrapper__"].minimized-ai
www.wikiwand.com##[class^="navbar_actions__"]
www.wikiwand.com##.wrapper_wrapper__jj00U.wrapper_footer__XQqx4, .wrapper_wrapper__jj00U.wrapper_footer__XQqx4 > .wrapper_container__51TyV
www.wikiwand.com##.footer_wrapper__eMIDC
www.wikiwand.com###ww-content > div.wrapper_wrapper__jj00U.wrapper_box__eMVrW:nth-of-type(1), 
###ww-content > div.wrapper_wrapper__jj00U.wrapper_box__eMVrW:nth-of-type(1) > .wrapper_container__51TyV
www.wikiwand.com##section.section_wrapper__8dcj4.top-section:nth-of-type(2) > .wrapper_wrapper__jj00U.wrapper_box__eMVrW, 
section.section_wrapper__8dcj4.top-section:nth-of-type(2) > .wrapper_wrapper__jj00U.wrapper_box__eMVrW > .wrapper_container__51TyV
www.wikiwand.com##html:style(font-size: 18px !important;)
www.wikiwand.com##body:style(font-size: 0.95rem !important;)
www.wikiwand.com##h1, h2, h3, h4, h5, h6:style(font-size: 1.2rem !important;)
www.wikiwand.com##p, li, td, th:style(font-size: 0.9rem !important;)
#
#
! === Redirects ===
wikipedia.org##+js(user-redirect-wikipedia.js)
```

Em custom scriptlets, adicione o seguinte js, salve com o nome de user-redirect-wikipedia.js

```javascript
'use strict';

(function() {
    const currentUrl = window.location.href;

    // Se a URL contém "oldformat=true", não redireciona
    if (currentUrl.includes('oldformat=true')) return;

    // Verifica se a URL corresponde a um artigo da Wikipedia
    const wikipediaRegex = /^https?:\/\/([a-z]{2,3})\.wikipedia\.org\/wiki\/([^#?]+)/;
    const match = currentUrl.match(wikipediaRegex);
    if (!match) return;

    const lang = match[1];
    const article = match[2]; // Já vem encoded da URL

    // Verifica tipo de conteúdo com HEAD
    fetch(currentUrl, { method: 'HEAD' })
        .then(response => {
            const contentType = response.headers.get('Content-Type');
            if (contentType && contentType.includes('text/html')) {
                // Remove o encodeURIComponent para evitar double encoding
                const newUrl = `https://www.wikiwand.com/${lang}/articles/${article}`;
                window.location.replace(newUrl);
            }
        })
        .catch(console.error);
})();
```

Na última linha do custom filter, ele invoca o JavaScript sempre que for acessado o link da Wikipedia, redirecionando para a Wikiwand.

Ou seja, não precisa de extensão alguma! Basta manter o Brave Shield ativado.

E ao abrir o link da wikiwand, ele aplica todos os bloqueios ali do custom filter.

Ficando a pagina:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1764525536588/3e6e57f4-034d-4337-a40e-1414b5a44a5e.png align="center")

### Filtros diretos na wikipedia

Outra alternativa ao usar wikiwand ou as extensões que mencionei acima de stylesheet e, se você tem tempo livre suficiente, pode ir alterando o formato da wikipedia direto nos filtros, sem precisar de scriptlets para redicionamento.

Cheguei a tentar algumas coisas, mas acabei indo pela praticidade do redirect.

Se quiser ver até onde cheguei:

```javascript
! === Wikipedia ===
wikipedia.org##.mw-page-header, .mw-footer, .mw-indicators, #siteSub, #jump-to-nav, 
.vector-page-toolbar-container, .vector-toc-pinned-container, 
.mw-portlet #p-tb, .vector-sitenotice-container, .mw-sidebar
pt.wikipedia.org##.mw-content-ltr.mw-parser-output > table.hp-kop:nth-of-type(1) > tbody > tr:nth-of-type(1) > td
pt.wikipedia.org##.mw-content-ltr.mw-parser-output > table.hp-kop:nth-of-type(1) > tbody > tr:nth-of-type(2) > td
pt.wikipedia.org###p-vector-user-menu-overflow > .vector-menu-content > .vector-menu-content-list
pt.wikipedia.org###vector-user-links-dropdown-checkbox
pt.wikipedia.org###vector-user-links-dropdown-label
pt.wikipedia.org###vector-appearance-dropdown-checkbox
pt.wikipedia.org###p-lang-btn-checkbox
pt.wikipedia.org###p-lang-btn-label
en.wikipedia.org###p-vector-user-menu-overflow > .vector-menu-content > .vector-menu-content-list
en.wikipedia.org###vector-appearance-dropdown-checkbox
en.wikipedia.org###vector-appearance-dropdown-label
en.wikipedia.org###p-lang-btn-checkbox
en.wikipedia.org###p-lang-btn-label
wikipedia.org##html:style(font-family: "Inter", "Segoe UI", "Helvetica Neue", sans-serif !important; font-size: 14px !important;)
wikipedia.org##body:style(max-width: 1900px !important; margin: auto !important; padding: 10px !important; line-height: 1.6 !important; background-color: #f9f9f9 !important;)
wikipedia.org##.mw-body:style(background: #ffffff !important; border-radius: 4px !important; box-shadow: 0 0 16px rgba(0,0,0,0.05) !important; padding: 20px !important;)
wikipedia.org##h1, h2, h3, h4, h5, h6:style(font-weight: 900 !important; margin-top: 1.5em !important; margin-bottom: 0.5em !important; font-size: 1.3rem !important;)
wikipedia.org##p, li:style(font-size: 0.95rem !important; margin-bottom: 0.75em !important;)
wikipedia.org##.mw-parser-output ul, .mw-parser-output ol:style(padding-left: 1.2em !important;)
wikipedia.org##table:style(border-collapse: collapse !important; width: 100% !important;)
wikipedia.org##th, td:style(border: 1px solid #ddd !important; padding: 4px !important; font-size: 0.9rem !important;)
wikipedia.org##img:style(border-radius: 8px !important; max-width: 100% !important;)
wikipedia.org##a:style(color: #1a73e8 !important; text-decoration: none !important;)
wikipedia.org##a:hover:style(text-decoration: underline !important;)
wikipedia.org##.box-More_citations_needed.plainlinks.metadata.ambox.ambox-content.ambox-Refimprove:style(
    margin-left: 0 !important; 
    margin-right: 0 !important; 
    max-width: 80% !important; 
    border-radius: 2px !important; 
    padding: 2px !important; 
    background-color: #fff8dc !important; 
    border: 1px solid #ffe599 !important; 
    box-shadow: none !important; 
    font-size: 0.9rem !important;)
en.wikipedia.org##.mw-footer-container
```