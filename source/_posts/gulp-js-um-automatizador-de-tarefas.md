title: 'gulp.js: Utilizando pela primeira vez'
tags:
  - Grunt
  - gulp.js
id: 21
categories:
  - Automatizadores de Tarefas
  - JavaScript
date: 2014-09-16 08:12:32
---

Hail, brothers! Hoje vim apresentar o gulp.js, o automatizador de tarefas que funciona exatamente como o seu nome sugere, um GOLE. É rápido, eficiente, simples de usar e configurar e hoje vocês verão por que <del>ele é muito melhor que o Grunt</del>.

## My name is gulp

### Instalando o gulp.js

Bom nós instalamos o gulp através do npm:

<pre class="lang:sh decode:true">npm install -g gulp</pre>

Após isto basta prosseguir para o próximo passo sem ter medo <del>da verdade</del>.

### Configurando o gulpfile.js

Assim como no Grunt, no gulp você precisa criar um arquivo chamado <span class="lang:default decode:true  crayon-inline ">gulpfile.js</span>  ou <span class="lang:default decode:true  crayon-inline ">Gulpfile.js</span> na pasta raiz que deseja iniciar as suas tarefas. A primeira coisa que deve fazer é... Okay! Vou deixar vocês chutarem:

a) Importar o gulp

b) Exportar uma função com o parâmetro gulp utilizando o module.exports do Node.js <del>para futuramente ser pegue pelo gulp-cli</del>

Isso mesmo, alternativa **a**! Bom, basta importar o gulp, dessa maneira:

<pre class="lang:default decode:true ">var gulp = require('gulp');</pre>

### Criando uma tarefa

Para criar uma tarefa no gulp, é muito simples <del>pois diferente do grunt, nós não temos que enfiar todas as tarefas dentro de um Object imenso</del> pois temos o <span class="lang:default decode:true  crayon-inline">gulp.task</span> . Deem uma olhada na sintaxe:

<pre class="lang:default decode:true">gulp.task('taskName', function () {
  // operations here
});</pre>

## Entendendo o gulp.js

Bom, o gulp.js é um plugin de node muuuuuuuuito simples, muito simples MESMO. Se pararmos para ler o [código fonte](https://github.com/gulpjs/gulp/blob/master/index.js) do gulp, veremos que temos várias funções, as mais utilizadas são <span class="lang:default decode:true  crayon-inline ">gulp.task</span> , <span class="lang:default decode:true  crayon-inline ">gulp.src</span> , <span class="lang:default decode:true  crayon-inline ">gulp.dest</span>  e <span class="lang:default decode:true  crayon-inline ">gulp.watch</span> . Já sabemos para que serve o nosso <span class="lang:default decode:true  crayon-inline ">gulp.task</span> , então creio que não haja necessidade de explicar para que ele serve, mas quanto ao resto, vou explicar abaixo para que cada uma dessas funções serve:

### gulp.src(caminhos[, opcoes])

O **gulp.src **como vimos no código-fonte do gulp.js, é simplesmente um wrapper ou envoltório para uma das funções do [vinyl-fs](https://github.com/wearefractal/vinyl-fs). Funciona da seguinte maneira, ele recebe uma String ou um Array através do parâmetro caminhos (com suporte a regex), e você pode passar também algumas opções através do próximo parâmetro, cujo qual deve ser um Object, confira as opções que você pode acrescentar bem aqui: [https://github.com/isaacs/node-glob#options](https://github.com/isaacs/node-glob#options&lt;strong&gt;&lt;/strong&gt;).

### gulp.watch(caminhos, tasks)

Serve para assistir arquivos, cada vez que qualquer desses arquivos mudam, todas as **tasks** (que deve ser um Array, mesmo que contenha apenas uma task) são executadas.

### gulp.dest(caminho)

Serve simplesmente para direcionar o seu arquivo que está sendo lido no stream para o **caminho**. (Entenderemos em breve)

## Exemplo

Bom, esta é a melhor parte, os EXEMPLOS de tarefas, aqui nós percebemos o quão simples funciona o automatizador de tarefas.

### Compilando os nossos estilos em SASS

Primeiro instalamos os plugins que usaremos (todos os plugins abaixo são pacotes do npm registry):

*   [gulp-sass](https://github.com/dlmanning/gulp-sass)
*   [gulp-rename](https://github.com/hparra/gulp-rename)
*   [gulp-cssmin](https://github.com/chilijung/gulp-cssmin/)

Importamos as nossas dependências no topo do nosso arquivo gulpfile.js, que ficará mais ou menos assim:

<pre class="lang:js decode:true ">var gulp = require('gulp');

var sass = require('gulp-sass');
var cssmin = require('gulp-cssmin');
var rename = require('gulp-rename');</pre>

Criamos uma nova tarefa:

<pre class="lang:default decode:true">gulp.task('stylesheets', function () {
});</pre>

Agora que já temos uma terefa, vamos utilizar o gulp.src e anexar a eles, os caminhos que desejamos colocar no stream:

<pre class="lang:default decode:true">gulp.task('stylesheets', function () {
  gulp.src('src/scss/**.{scss,sass}')
});</pre>

Agora a partir daqui nos começaremos a utilizar o .pipe() logo após o gulp.src(), e basta que anexemos os nossos plugins dentro do pipe e pronto, já estamos executando. Vejam como é simples:

<pre class="lang:default decode:true">gulp.task('stylesheets', function () {
  gulp.src('src/scss/**.{scss,sass}')
    .pipe(sass())
    .pipe(cssmin())
    .pipe(rename({
      basename: 'style',
      suffix: '.min',
      extname: '.css'
    }))
});</pre>

Agora tudo  o que precisamos fazer é jogar o nosso <span class="lang:default decode:true  crayon-inline ">gulp.dest</span>  ao final de tudo para dar um destino ao nosso novo arquivo que por enquanto, encontrasse vagando solitário dentro das pastas temporárias dos nossos computadores. Da seguinte maneira:

<pre class="lang:default decode:true">gulp.task('stylesheets', function () {
  gulp.src('src/scss/**.{scss,sass}')
    .pipe(sass())
    .pipe(cssmin())
    .pipe(rename({
      basename: 'style',
      suffix: '.min',
      extname: '.css'
    }))
  .pipe(gulp.dest(path.join(__dirname, 'public', 'css')));
});</pre>

Como utilizamos o** gulp-rename**, então não precisamos nos preocupar com o nome do arquivo.

Bom, espero que tenham entendido <del>que o gulp é bem melhor que o grunt </del>tudo, quaisquer dúvidas, reclamações etc. deixem seus comentários and see you, buddies. ;)