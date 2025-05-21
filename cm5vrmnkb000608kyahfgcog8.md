---
title: "Construindo um simples web server em Vlang"
seoTitle: "Crie um Web Server Simples com Vlang"
seoDescription: "Aprenda a configurar um simples servidor web usando a linguagem V, explorando frameworks como vweb, veb e picoev"
datePublished: Tue Jan 14 2025 00:59:39 GMT+0000 (Coordinated Universal Time)
cuid: cm5vrmnkb000608kyahfgcog8
slug: construindo-um-simples-web-server-em-vlang
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/XJXWbfSo2f0/upload/9084cc4e6fd607b8f18b920e3126e5ad.jpeg
tags: web-servers, framework, v, vlang, v-language, v-lang

---

Vamos subir um web server usando a linguagem V.

Caso não conheça a Vlang, aqui tem um guia para aprender a programar em V:

%[https://esli.blog.br/introducao-a-linguagem-de-programacao-v-guia-completo] 

As vantagens de utilizar V para a Web:

* Uma linguagem segura, rápida, com a agilidade de desenvolvimento de Python ou Ruby e o desempenho de C.
    
* Zero dependências: tudo o que é necessário para o desenvolvimento web vem com a linguagem num pacote de 1 MB.
    
* Binários muito pequenos: um site estático no tutorial do vweb tem cerca de 150 KB.
    
* Fácil implementação: um único arquivo binário inclui até os modelos pré-compilados.
    
* Funciona no hardware mais barato com um custo mínimo: para a maioria das aplicações, uma instância de 3 dólares é suficiente.
    
* Desenvolvimento rápido, sem qualquer tipo de cliché.
    

As maneiras de subir um web server usando V:

* ***vweb*** - descontinuado, evoluiu para o veb (abaixo)
    
    Source: [https://github.com/vlang/v/tree/master/vlib/vweb](https://github.com/vlang/v/tree/master/vlib/vweb)
    
    Na doc oficial tem um tutorial sobre como subir um site estático em 400 Kb e com SQlite. Link: [https://github.com/vlang/v/tree/master/tutorials/building\_a\_simple\_web\_blog\_with\_vweb](https://github.com/vlang/v/tree/master/tutorials/building_a_simple_web_blog_with_vweb)
    

* ***veb*** - Um framework built-in (nativo/integrado ao V), similar ao Python Flask
    
    Source: [https://github.com/vlang/v/tree/master/vlib/veb](https://github.com/vlang/v/tree/master/vlib/veb)
    

* ***picoenv*** - Um pequeno e rápido loop de eventos para aplicações de rede
    
    Source: [https://github.com/vlang/v/tree/master/vlib/picoev](https://github.com/vlang/v/tree/master/vlib/picoev)
    
    Module page: [https://modules.vlang.io/picoev.html](https://modules.vlang.io/picoev.html)
    

* ***Vex*** - Framework web modular
    
    Source: [https://github.com/nedpals/vex](https://github.com/nedpals/vex)
    
    Passo a passo de uso: [https://blog.vlang.io/vex-showcase/](https://blog.vlang.io/vex-showcase/)
    
    wiki: [https://nedpals.github.io/vex/](https://nedpals.github.io/vex/)
    

# vweb

Com o V já instalado e testado (acesse o tutorial sobre como instalar o Vlang em [https://esli.blog.br/introducao-a-linguagem-de-programacao-v-guia-completo#heading-instalando-e-atualizando-o-v](https://esli.blog.br/introducao-a-linguagem-de-programacao-v-guia-completo#heading-instalando-e-atualizando-o-v) )

Vamos criar o projeto:

`v new site`

Ele criará toda estrutura e já com o git local repo iniciado, basta apenas adicionar o remote server (origin) no git.

```bash
ls -lhtra site
drwxr-xr-x. 1 esli esli  12 jan 13 19:22 src
drwxr-xr-x. 1 esli esli   8 jan 13 19:22 ..
-rw-r--r--. 1 esli esli 107 jan 13 19:22 v.mod
-rw-r--r--. 1 esli esli 173 jan 13 19:22 .gitattributes
-rw-r--r--. 1 esli esli 123 jan 13 19:22 .editorconfig
drwxr-xr-x. 1 esli esli  98 jan 13 19:22 .git
-rw-r--r--. 1 esli esli 239 jan 13 19:22 .gitignore
```

```bash
git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .editorconfig
        .gitattributes
        .gitignore
        src/
        v.mod

nothing added to commit but untracked files present (use "git add" to track)
```

Lá em src/ crie o `server.v`

```c
module main

import vweb

struct App {
	vweb.Context
}

fn main() {
	app := App{}
	vweb.run(app, 8080)
}

@['/index']
pub fn (mut app App) index() vweb.Result {
	return app.text('Hello world from vweb!')
}
//access the endpoint on http://localhost:8080/


fn (mut app App) test() vweb.Result {
    return app.text('Hello world test path from vweb!')
}
//access the endpoint on http://localhost:8080/test

fn (mut app App) page() vweb.Result {
	message := 'Hello, world from page Vweb!'
    return $vweb.html()
}
//access the endpoint on http://localhost:8080/page
```

Agora é só compilar e rodar!  
Execute o comando:

`v run src/server.v`

```c
v run src/server.v
[Vweb] Running app on http://localhost:8080/
[Vweb] We have 7 workers
```

E com ele em execução, acesse: http://localhost:8080 e http://localhost:8080/test

Criei também um http://localhost:8080/page, na qual ele mostrará o conteúdo de src/page.html no navegador.

Caso altere, precisará reiniciar a execução. Para rodar no modo live-reload, e ter o update da web ‘ao vivo’ (ou caso o conteúdo não seja estático), utilize o comando:

`v watch run src/server.v`

Pronto!  
Rodamos um web server usando o vweb (descontinuado, mas ainda funcional), expondo um texto simples, dois paths e sendo um deles, uma página web simples.

# veb

Com o veb, foi criado, por exemplo, o tVeb, Tiniest Veb Server. Um Web Server completo usando a veb com um binário completo em menos de 1Mb focado em arquivos estáticos.

%[https://github.com/davlgd/tVeb] 

Mesmo assim, vamos subir um simples web server com a lib veb:

Crie o `src/server-veb.v`

```c
module main

import veb

pub struct Context {
	veb.Context
}

pub struct App {}

pub fn (app &App) index(mut ctx Context) veb.Result {
	return ctx.text('Hello, World from veb!')
}

fn main() {
	mut app := &App{}
	veb.run[App, Context](mut app, 8181)
}
```

E para rodar em live reload, use o comando: `v -d veb_livereload watch run src/server-veb.v`  
  
Pronto!

A documentação completa do veb: [https://modules.vlang.io/veb.html](https://modules.vlang.io/veb.html)

# picoev

O picoev é uma implementação V do [picoev](https://github.com/kazuho/picoev) feito em C, que é "Um pequeno e rápido loop de eventos para aplicações de rede".

A documentação do picoenv: [https://modules.vlang.io/picoev.html](https://modules.vlang.io/picoev.html)

```c
import json
import picoev
import picohttpparser

const port = 8282

struct Message {
	message string
}

@[inline]
fn json_response() string {
	msg := Message{
		message: 'Hello, World from pico!'
	}
	return json.encode(msg)
}

@[inline]
fn hello_response() string {
	return 'Hello, World from pico!'
}

fn callback(data voidptr, req picohttpparser.Request, mut res picohttpparser.Response) {
	if req.method == 'GET' {
		if req.path == '/t' {
			res.http_ok()
			res.header_server()
			res.header_date()
			res.plain()
			res.body(hello_response())
		} else if req.path == '/j' {
			res.http_ok()
			res.header_server()
			res.header_date()
			res.json()
			res.body(json_response())
		} else {
			res.http_ok()
			res.header_server()
			res.header_date()
			res.html()
			res.body('Hello, World from pico!\n')
		}
	} else {
		res.http_405()
	}
	res.end()
}

fn main() {
	println('Starting webserver on http://localhost:${port}/ ...')
	mut server := picoev.new(port: port, cb: callback)!
	server.serve()
}
```

Novamente, só executar o comando: `v watch run src/server-pico.v`

# Conclusão

Criamos 3 web servers:

http://localhost:8080, http://localhost:8080/test e http://localhost:8080/page - usando vweb

http://localhost:8181 - usando veb

http://localhost:8282 - usando picoenv

O código destes testes estão em [https://github.com/Esl1h/static-v-site](https://github.com/Esl1h/static-v-site) e [https://gitlab.com/Esl1h/static-v-site](https://gitlab.com/Esl1h/static-v-site)

Em todos eles, a questão do TLS/SSL pode ser resolvida com proxy reverso usando NGINX ou Caddy.

Faltou explorarmos o framework Vex e também criar algo com a lib net.http (que não mencionei anteriormente e sua doc está em [https://modules.vlang.io/net.http.html](https://modules.vlang.io/net.http.html)).

Ficará para um próximo post.