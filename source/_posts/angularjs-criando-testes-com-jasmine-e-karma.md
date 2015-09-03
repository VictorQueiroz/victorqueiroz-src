title: 'AngularJS: Criando testes com Jasmine e Karma'
tags:
  - Jasmine
  - JavaScript
  - Testes unitários
id: 45
categories:
  - AngularJS
  - Frameworks
  - JavaScript
date: 2014-09-18 00:33:04
---

> Antes de mais nada é importante que você leia o artigo [Jasmine: Primeiro contato com testes unitários](http://victorqueiroz.co/jasmine-primeiro-contato-com-testes-unitarios/), caso ainda não saiba o que é Jasmine.

<!--more-->
## Dependências

*   [angular-mocks](https://docs.angularjs.org/api/ngMock)

## Escrevendo testes

É bem simples, é importante que você utilize o <span class="lang:js decode:true  crayon-inline">beforeEach()</span>  do Jasmine, o <span class="lang:js decode:true  crayon-inline ">module</span>  e o <span class="lang:js decode:true  crayon-inline">inject</span>  do **angular-mocks**, e o <span class="lang:js decode:true  crayon-inline ">$injector</span>  do próprio angular para carregar todas as dependências necessárias.

Veja um simples exemplo em que eu carrego um serviço de dentro de um módulo:
<pre class="lang:js decode:true ">describe('app module', function () {
  var User;

  beforeEach(module('app'));

  beforeEach(inject(function ($injector) {
    User = $injector.get('User');
  }));

  it('should have a User service', function () {
    expect(typeof User.$get).toEqual('function');
  });
});</pre>
Vejam que na linha 4 nós carregamos o módulo, é importante lembrar que caso este módulo utilize serviços de um outro módulo, ele terá que ser carregado também, basta adicionar abaixo do <span class="lang:js decode:true  crayon-inline ">beforeEach</span>  em que carregamos o módulo principal e o seu problema estará resolvido.

Na linha 6, nós injetamos através do <span class="lang:js decode:true  crayon-inline ">inject</span> o <span class="lang:js decode:true  crayon-inline ">$injector</span>  na função, o <span class="lang:js decode:true  crayon-inline">inject</span> pode ser utilizado em qualquer lugar do seu teste. A partir daí você poderá importar o <span class="lang:js decode:true  crayon-inline">$compile</span> caso queira compilar diretivas no DOM, o <span class="lang:js decode:true  crayon-inline ">$rootScope</span> caso queira criar um <span class="lang:js decode:true  crayon-inline ">$scope</span> para compilar as diretivas.

## Rodando seus testes com o Karma Runner

### Instalando o Karma

Você deve instalar o Karma via npm globalmente:
<pre class="lang:sh decode:true">npm instal -g karma</pre>
E localmente no seu projeto (ou ele não irá funcionar):
<pre class="lang:sh decode:true">npm install --save-dev karma</pre>
Criando um arquivo de configuração do Karma

É algo simplesmente simples (?) e rápido! Vá até a pasta raiz do seu projeto e digite:
<pre class="lang:sh decode:true">karma init</pre>
Preencha todas as perguntas e BUM, você tem um arquivo de configuração do Karma.

### Rodando os testes com o Karma

Mais uma das maravilhas do CLI do Karma:
<pre class="lang:sh decode:true ">karma start --no-single-run --log-level debug</pre>
Você pode trocar o parâmetro <span class="lang:sh decode:true  crayon-inline ">--no-single-run</span>  por <span class="lang:sh decode:true  crayon-inline ">--single-run</span> , e ele não ficará assistindo as mudanças que ocorrerão nos seus arquivos para reiniciar os testes, inclusive é importante deixar que o padrão seja <span class="lang:sh decode:true  crayon-inline ">--single-run</span>  para caso tenha planos de usar algum sistema de **Continuous Integration**, caso contrário, ele pode retornar como se os testes estivessem falhados quando ele apenas parou os testes para assistir as mudanças dos arquivos.

### Finalização e dicas

Tenha em mente de que você terá que configurar o seu arquivo de configuração do Karma (geralmente karma.conf.js, localizado na pasta raiz do seu projeto) para carregar o **angular** e o **angular-mocks**, e todos os outros assets que você carrega para fazer seus módulos funcionarem corretamente, é claro.

O seu arquivo de testes deve ser parecido com o do exemplo acima, e qualquer dúvida você pode ler a documentação do angular-mocks OU pode vir aqui e deixar o seu comentário e eu terei um imenso prazer em lhe responder.

Quaisquer dúvidas, deixem abaixo nos comentários ou enviem-me e-mails, um abraço e até a próxima!