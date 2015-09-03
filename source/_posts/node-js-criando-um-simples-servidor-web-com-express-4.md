title: Node.js - Criando um simples servidor web com Express 4
tags:
  - Fácil
  - JavaScript
  - Node.js
  - Servidor
id: 15
categories:
  - Express
  - Frameworks
  - Node.js
date: 2014-09-16 04:59:50
---

&nbsp;

Assim como eu, provavelmente você já tenha feito a si essa pergunta:

> Eu posso criar um simples servidor web utilizando Node.js? Eu só quero algo pra rodar arquivos HTML, carregar os roteiros de códigos de maneira fácil.

**Resposta: **Sim, você pode fazer isso com Node.

Tendo em mente que você deve executar todos esses passos de dentro da pasta do seu projeto. Segue com a gente o passo a passo e veja como é simples!

<!--more-->
### Criando o package.json (opcional)

É apenas simples e fácil assim como o próximo passo. Vá até a linha de comando e digite:

<pre class="lang:sh decode:true ">npm init</pre>

Preencha todas as perguntas e vá para o próximo passo.

### Criando o bower.json (opcional)

Tão simples quanto criar o **package.json**, ainda sem sair da linha de comando, digite:

<pre class="lang:sh decode:true ">bower init</pre>

Preencha mais uma vez todas as perguntas.

### Instalando as nossas dependências com o npm

Para fazer o servidor rodar nós precisaremos do express e nada mais. Então:

<pre class="lang:sh decode:true">npm install --save express</pre>

### Criando um arquivo para iniciar o servidor

Agora crie um arquivo para iniciar o servidor, no nosso exemplo o arquivo se chama **server.js**, importe o **Express** e crie a aplicação, dessa maneira:

<pre class="lang:js decode:true">var express = require('express');
var app = express();

var path = require('path');

app.use(express.static(path.join(__dirname, 'public')));

app.listen(3000);</pre>

#### Entendendo o código

Na primeira linha estamos pedindo a dependência <span class="lang:default decode:true  crayon-inline ">express</span> , na segunda linha estamos iniciando a aplicação, na quarta estamos pedindo a dependência <span class="lang:default decode:true  crayon-inline">path</span>** **(que faz parte do núcleo do Node.js).

##### Linha 6

Antes precisamos entender para que servem o <span class="lang:js decode:true  crayon-inline ">app.use()</span>  e o <span class="lang:js decode:true  crayon-inline ">express.static()</span> :

De acordo com a documentação do Express, o app.use() é utilizado para montagem de middlewares, se não definirmos nenhum caminho (<span class="lang:js decode:true  crayon-inline ">app.use('/someUrlHere', some.Middleware())</span> ) antes de declararmos o middleware, que no caso é o <span class="lang:js decode:true  crayon-inline">express.static()</span>, por padrão ele será definido no caminho raiz, ou seja, quando determinamos a pasta **public** na linha 6, não estamos dizendo que aquela é a pasta pública e que ela vai conter o nosso site, estamos dizendo que quando o usuário entrar no caminho raiz do site, ele estará acessando a esta pasta também, OU SEJA, todos as pastas e arquivos dentro dela estarão disponíveis para o acesso diretamente do navegador.

Já o <span class="lang:js decode:true  crayon-inline ">express.static</span> é baseado no [server-static](https://github.com/expressjs/serve-static) e serve simplesmente para prover o acesso em uma pasta e todos os seus arquivos.

##### Última linha

Nesta linha, estamos apenas iniciando o servidor da aplicação.

### Iniciando o servidor

Voltando novamente a linha de comando, digite:

<pre class="lang:sh decode:true">node server.js</pre>

### Dicas

Você pode por outras pastas no caminho raiz do servidor web de sua aplicação para facilitar o acesso aos assets, como por exemplo, a pasta <span class="lang:default decode:true  crayon-inline ">bower_components</span> , você pode simplesmente digitar a seguinte linha após ou antes da linha 6 no nosso arquivo server.js:

<pre class="lang:js decode:true ">app.use('/lib', express.static(path.join(__dirname, 'bower_components')));</pre>

E pronto, a rota **/lib **proverá o acesso para todas pastas e arquivos da respectiva pasta, ou seja, ao invés de ter que copiar os arquivos para dentro da pasta **public**, você pode simplesmente, acessar [http://localhost:3000/lib/angular/angular.min.js](http://localhost:3000/lib/angular/angular.min.js) e vu alá, você tem acesso ao asset que procurava sem ter que fazer nada de muito complicado.

Bom, espero que tenham gostado, até a próxima! :)