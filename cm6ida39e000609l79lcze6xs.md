---
title: "Técnicas para Lidar com erros em Vlang Utilizando os.system e Operador or"
seoTitle: "Lidar com Erros em Vlang: Dicas Práticas"
seoDescription: "Aprenda como lidar com erros em Vlang utilizando os.system e o operador or para automatizar configurações com sucesso"
datePublished: Wed Jan 29 2025 20:36:41 GMT+0000 (Coordinated Universal Time)
cuid: cm6ida39e000609l79lcze6xs
slug: tecnicas-para-lidar-com-erros-em-vlang-utilizando-ossystem-e-operador-or
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ieic5Tq8YMk/upload/097def263055cb1eec5cdb7787d97556.jpeg
tags: error, linux, if-else, vlang, v-language, v-lang

---

No meu projeto (hobby) de automatizar as configurações e restore numa nova instalação de S.O (Fedora ou Debian like), resolvi (por tempo livre) transformá-lo de Shell Script para Vlang.  
  
E nisto encontrei alguns desafios.

Um exemplo sobre tratar erro/gerar logs:

A function `install_fonts` em Vlang:

```c
fn install_fonts() {
	local_fonts_dir := os.home_dir() + '/.local/share/fonts'
	if !os.exists(local_fonts_dir) {
			os.mkdir_all(local_fonts_dir) or { 
                println('Failed to create ~/.local/share/fonts directory') 
                return }
			}
}
```

Ele move o diretório para `/.local/share/fonts` na home `~` do usuário, e logo abaixo uso o bloco if para validar se o path não existe e cria-lo.

Nesta validação, ele retorna um valor do tipo Result

  
E com o operador `or { ... }` tento tratar algum erro colocando um `println` na tela caso falhe na criação do path e um return no final.

Porém o `or` só funciona com resultados do tipo (type) Result ou Option.  
**Result**: um valor que pode estar presente ou um erro. Pode ser representado por **!**

**Option**: um valor que pode ou não estar presente. Pode ser representado por **?**

Exemplo de Option type:

```c
fn func() ?int {
    return none
}
```

Exemplo de Result type:

```c
fn func() !int {
    return error('Some error')
}
```

Ao manipular esses tipos, você pode usar o bloco `or` para manipular a ausência de um valor ou um erro. Por exemplo:

```c
a := func() or { "default" } 
```

A variável `a` será "default" se func() retornar none ou error

Novamente, o operador `or` só pode ser usado com funções que retornam `Option` ou `Result`.

Mas lá naquele script em Shell/Bash, a maioria das execuções é de comandos no sistema, logo, precisarei usar a função `os.system()` para executá-los.

### E eis o problema: o os.system vai retornar um valor inteiro devido a execução com sucesso do comando no linux.

Qualquer execução de comando no Linux retorna um número inteiro, que pode ser verificado com `$?`. O valor `0` indica sucesso, enquanto valores diferentes de `0` indicam falhas, com diferentes códigos de erro dependendo do comando. Além do output no terminal de quem está executando.

Para conseguir tratar erros neste caso (exibir uma mensagem caso falhe a execução do comando do linux via Vlang), uma alternativa a seguir é:

```c
exit_code := os.system('wget -c ${fonts_pack} -P ${local_fonts_dir}') 

if exit_code != 0 { 
		println('Failed to download fonts.zip') 
		return 
		}
```

Movo a execução do `os.system()` para a variável `exit_code`, e depois, num bloco if, com isto, pode-se validar se retorna ou não com sucesso (type `result`) usando o `or`.

E com isto, temos o tratamento de erro e a exibição de mensagens em caso de erro em todas as etapas da execução:

```c
fn install_fonts() { 
		local_fonts_dir := os.home_dir() + '/.local/share/fonts' 
		if !os.exists(local_fonts_dir) { 
				os.mkdir_all(local_fonts_dir) or {
						println('Failed to create ~/.local/share/fonts directory') 
						return 
						} 
		} 
		
		hack_fonts := 'https://github.com/ryanoasis/nerd-fonts/releases/download/v2.2.2/Hack.zip' 
		installhack := os.system('wget -c ${hack_fonts} -P ${local_fonts_dir}') 
		
		
		if installhack != 0 { 
				println('Failed to download Hack.zip') 
				return 
		} 
		
		println('Hack.zip downloaded and saved to ${local_fonts_dir}') }
```

É isso…

Caso queira ver todo o código, siga em

%[https://github.com/Esl1h/UAI-FAI]