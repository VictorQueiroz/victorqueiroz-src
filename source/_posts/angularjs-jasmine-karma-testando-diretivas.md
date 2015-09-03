title: 'AngularJS, Jasmine, Karma: Testando diretivas'
tags:
  - AngularJS
  - directives
  - Diretivas
  - Jasmine
  - JavaScript
  - Teste de unidade
  - Testes unitários
id: 56
categories:
  - AngularJS
  - Frameworks
  - JavaScript
  - Testes unitários
date: 2014-09-25 08:03:09
---

Se você ainda não teve a experiência de usar o ngMock com Jasmine, não sabe o que está perdendo, é tudo muito maravilhoso e poderoso. Continue lendo, vamos lá!

Como falei no final do artigo [AngularJS: Criando testes com Jasmine e Karma](http://victorqueiroz.co/angularjs-criando-testes-com-jasmine-e-karma/), você pode usar o $injector do AngularJS para chamar em qualquer lugar tudo aquilo que precisamos para trabalhar com nossos módulos, neste guia nós utilizaremos as seguintes dependências:

*   $injector
*   $rootScope
*   $compile

## Importando as dependências

Vamos começar chamando as nossas dependências dentro do nosso teste:
<pre class="lang:js decode:true">describe('app module', function () {
	var $compile, $rootScope;

	beforeEach(module('app'));

	beforeEach(inject(function ($injector) {
		$compile = $injector.get('$compile');
		$rootScope = $injector.get('$rootScope');
	}));
});</pre>
Logo acima importamos as dependências que serão reutilizadas pelas próximas suítes, agora vamos importar as dependências da nossa diretiva:
<pre class="lang:js decode:true">describe('app module', function () {
	var $compile, $rootScope;

	beforeEach(module('app'));

	beforeEach(inject(function ($injector) {
		$compile = $injector.get('$compile');
		$rootScope = $injector.get('$rootScope');
	}));

	describe('replacer directive', function () {
		var element, scope, form;

		beforeEach(inject(function ($injector) {
			scope = $rootScope.$new();
		}));
	});
});</pre>
Agora precisamos compilar o nosso elemento dentro da suíte <span class="lang:js decode:true  crayon-inline ">replacer directive</span> , faremos da seguinte maneira:
<pre class="lang:js decode:true">	describe('replacer directive', function () {
		var element, scope, form;

		beforeEach(inject(function ($injector) {
			scope = $rootScope.$new();
			element = '&lt;form&gt;' + '&lt;input ng-model="text" name="text" replacer-directive&gt;' + '&lt;/form&gt;';
			element = angular.element(element);
			element = $compile(element)(scope);
		}));
	});</pre>
Agora você deve estar se perguntando:
> Como diabos eu vou pegar o meu input? Vou ter que fazer malabarismos com jQuery ou document.querySelector?
**Resposta: **NÃO! Basta dar um valor para a única variável que ainda não definimos dentro da suíte <span class="lang:js decode:true  crayon-inline ">replacer directive</span> :
<pre class="lang:js mark:7 decode:true">beforeEach(inject(function ($injector) {
	scope = $rootScope.$new();
	element = '&lt;form&gt;' + '&lt;input ng-model="text" name="text" replacer-directive&gt;' + '&lt;/form&gt;';
	element = angular.element(element);
	element = $compile(element)(scope);
        form = scope.form;
        scope.$digest();
}));</pre>
Agora podemos acessar ao nosso input através da variável <span class="lang:js decode:true  crayon-inline ">form</span> , da seguinte forma:
<pre class="lang:js decode:true ">form.text</pre>
Note que utilizamos um novo método na penúltima linha, o <span class="lang:js decode:true  crayon-inline ">scope.$digest()</span> , ele cuidará de atualizar o scope, todas as modificações que fizer no scope deve conter um <span class="lang:js decode:true  crayon-inline ">scope.$digest()</span> após para que todas as modificações sejam aplicadas ao DOM.

Agora nós podemos fazer os nossos testes:
<pre class="lang:js decode:true">it('should replace 2 with 3', function () {
  scope.text = 252;
  scope.$digest();

  expect(form.text).toBe(352);
});</pre>
A partir daí podemos ser felizes para sempre e testar as nossas diretivas de todas as maneiras possíveis para que tudo funcione exatamente como esperado.

Como já sabem, deixem suas dúvidas nos comentários. Um abraço a todos e até a próxima!