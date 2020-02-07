# R028 - Estruturando Exercícios

**Nessa aula foi montado uma estrutura para conseguir aplicar conceitos básicos de HTML e JavaScipt**
- Criado um arquivo chamado *index.html*
- Criado uma pasta chamada *exercicios*
- Dentro de **exercicios**, criou-se dois arquivos: *temp.html* e *teste.html*

<br>

### No arquivo index.html
Criou uma estrutura base

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Exercícios HTML</title>
</head>

<body>
    <header>
        <h1>Exercícios HTML</h1>
    </header>

    <nav>
        <a href="exercicios/teste.html">00 - Teste</a>
        <a href="exercicios/temp.html">TEMP</a>
    </nav>

    <section id="conteudo"></section>

    <footer>
        <br>
        Curso de Web Moderno
    </footer>
</body>

</html>
```

Após isso, criamos os códigos dos outros arquivos html

*teste.html*:

```html
<h1>Funcionou</h1>
```

*temp.html*:
 
```html
<h1>Teste</h1>
```

<br>

Agora precisamos criar o **Script** para que o próprio browser não faça a **requisição** automática em relação as tag's `<a>`. Assim vamos utilizar funcionalidades **Ajax** para que faça o conteúdo das outras páginas serem embutidas na *index.html* na tag `<section>`.

```html
<script>
    document.querySelectorAll('a').forEach(link => {
        const conteudo = document.getElementById(
            'conteudo'); // Como esta pegando pelo id, nao precisa do '.'

        link.onclick = function (e) {
            e.preventDefault(); // Previni que a navegacao ocorra

            fetch(link.href) // XML HTTP Request, baseado em Promise para fazer requisicao AJAX
                .then(resp => resp.text()) //resp.text pega o conteudo HTML
                .then(html => conteudo.innerHTML =
                    html) //Substituição da section para o texto que tem do link.href - text - html
        }
    })
</script>
```

Na primeira linha `document.querySelectorAll('a').forEach(link => {`, estamos pegando todos os elementos que contenham a tag `<a>` e na função `forEach`, para cada laço, estamos passando na variável **link** os elementos, para assim manipular da forma que queremos.

<br>

Na segunda linha `const conteudo = document.getElementById('conteudo');`, estamos pegando o elemento que contenha o id **conteudo**

<Br>

Agora começaremos a função que vai trazer o conteúdo da outra página para a *index.html*
Primeiro atribuimos no elemento <a> direcionado ao evento `onClick()` a uma **função closure**
`function(e)` tem um parâmetro que o próprio evento passa, assim, precismos evitar que a funcionalidade padrão dele seja executada. Para isso usamos `e.PreventDefault()`.
Nesse momento vamos precisar fazer a requisição ajax, feita por uma função nova JavaScript, o **fetch**. Ela retorna uma **Promise**, assim, em seu retorno podemos usar o `then`. 
No parâmetro do fetch, passamos a **url** ou o **link.href**. Agora podemos pegar todo o conteúdo com o `resp.text`. Só precisamos fazer a substituição da *section* para esse novo conteúdo, assim utilizamos o **innerHTML** atribuindo o html.

<br>

### Podemos deixar o código melhor

Não é muito legal deixar pegar o elemento direto da tag, que no caso é o `<a>`. Assim podemos usar uma propriedade personalizada. Deixamos o `href` para fazer com que o ponteiro do mouse muda ao passar por cima do link, mas criamos uma nova propriedade passando a url. `<a href wm-nav="exercicios/teste.html">00 - Teste</a>`. Nesse caso, `wm-nav` é quem será responsável.
Agora precisamos arrumar a função. Na linha para pegar o elemento `a`, temos que mudar o `document.querySelectorAll('a').forEach(link =>` para 
`document.querySelectorAll('[wm-nav]').forEach(link =>`.
Outro lugar para alterar é no `link.href`, agora precisamos utilizar o `link.getAttribute('wm-nav')` para pegar o conteúdo que está nessa propriedade.