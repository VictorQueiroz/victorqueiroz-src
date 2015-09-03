title: 'AngularJS: Diretivas - Saiba o que são e como elas funcionam'
tags:
  - AngularJS
  - Diretivas
  - Google
  - JavaScript
id: 9
categories:
  - AngularJS
  - Frameworks
  - JavaScript
date: 2014-09-16 03:07:52
---

### [![AngularJS-large](http://victorqueiroz.co/wp-content/uploads/2014/09/AngularJS-large.png)](http://victorqueiroz.co/wp-content/uploads/2014/09/AngularJS-large.png)O que são as diretivas?

Diretivas ou directives (em inglês), são marcadores em um [DOM](http://en.wikipedia.org/wiki/Document_Object_Model) element (podendo ser um atributo, nome de elemento, comentário ou classe das [CSS](http://en.wikipedia.org/wiki/Cascading_Style_Sheets))

que fala para o Angular [**HTML compiler**](https://docs.angularjs.org/guide/compiler) ([$compile](https://docs.angularjs.org/api/ng/service/$compile)) anexar um designado conteúdo, ou até mesmo transformar/gerenciar o DOM e suas "childrens".

O Angular em si já vem com várias dessas diretivas embutidas. Tais como `ngView`, `ngModel`, `ngBind`, `ngRepeat`, `ngShow`, `ngHide`. Assim como você pode criar os seus controllers/controladores, você também pode criar as suas próprias diretivas para usar no Angular.

### Usando diretivas

Antes de escrevermos uma diretiva, precisamos saber como o Angular [HTML compiler](https://docs.angularjs.org/guide/compiler) determina quando usar uma dada diretiva.

No exemplo abaixo, nós estamos dizendo que o elemento `&lt;input&gt;` corresponde a diretiva `ngModel`.

<pre class="lang:js decode:true ">&lt;input type="text" ng-model="foo"&gt;</pre>

O exemplo abaixo também corresponde a diretiva `ngModel`:

<div>
<pre class="lang:js decode:true">&lt;input type="text" data-ng:model="foo"&gt;</pre>
</div>

Nos referimos a diretivas pelo seu nome **normalizado** (i.e. `ngRepeat`, `minhaDiretivaNumeroUm`) em case-sensitive e em [CamelCase](http://pt.wikipedia.org/wiki/CamelCase). Contudo, já que o HTML é case-insensitive as diretivas precisam ter seus nomes desnormalizados ou adaptados antes de entrarem no DOM.

#### Desnormalização

Existem vários processos de desnormalização, um deles é o processo de desnormalização delimitado por traços, que se baseia em substituir as letras maíusculas por letras minúsculas e adicionar um traço antes (.i.e `ngRepeat`, `ngModel`, `myDirective`, se transformam respectivamente em `ng-repeat`, `ng-model` e `my-directive` dentro do DOM), porém é possível adicionar prefixos e outros parâmetros.

#### Normalização

O processo de normalização serve para traduzir o nome desnormalizado no nome real da diretiva.

1.  É retirado o x- e data- do começo do element/atributo.
2.  :, - ou _-x são convertidos em camelCase.

Onde x no passo DOIS, representa o nome da classe em formato de dash-delimited.

Ou seja, `ng-model`, `my-directive` ou `this-is-a-directive` são traduzidos para `ngModel`, `myDirective` ou `thisIsADirective`, o contrário do que foi explicado anteriormente, no processo de desnormalização.

Abaixo você pode ver um exemplo com as combinações possíveis para a diretiva `ngBind`.

<div>
<pre class="lang:default decode:true">&lt;div ng-controller="AppController"&gt;
  Hello &lt;input ng-model='name'&gt; &lt;hr/&gt;
  &lt;span ng-bind="name"&gt;&lt;/span&gt; &lt;br/&gt;
  &lt;span ng:bind="name"&gt;&lt;/span&gt; &lt;br/&gt;
  &lt;span ng_bind="name"&gt;&lt;/span&gt; &lt;br/&gt;
  &lt;span data-ng-bind="name"&gt;&lt;/span&gt; &lt;br/&gt;
  &lt;span x-ng-bind="name"&gt;&lt;/span&gt; &lt;br/&gt;
&lt;/div&gt;</pre>
</div>

O `$compile` suporta diretivas baseadas em nomes de elementos, atributos, classes das CSS e comentários.

Abaixo você pode ver alguns métodos que você pode utilizar para marcar diretivas:

<div>
<pre class="lang:default decode:true">&lt;my-dir&gt;&lt;/my-dir&gt;
&lt;span my-dir="exp"&gt;&lt;/span&gt;
&lt;!-- directive: my-dir exp --&gt;
&lt;span class="my-dir: exp;"&gt;&lt;/span&gt;</pre>
</div>

### Vinculando variáveis em atributos

Em breve!

### Criando diretivas

Primeiro vamos falar a respeito da API utilizada para criação de diretivas. Assim como os controladores, as diretivas são registradas em módulos. Para registrar uma diretiva, você se usa `module.directive`.

O module.directive deverá ser aplicado da forma como esta escrito abaixo:

<div>
<pre class="lang:default decode:true">angular.module('app', [])

.directive('validate', function() {
  return {
    template: 'Esta é a minha primeira diretiva, e eu ainda estou aprendendo.'
  };
});</pre>
</div>

A definição técnica é `module.directive('directiveNormalizedName', directiveFunction)`, a função aplicada, deverá retornar um objeto.

#### Restricts

As restricts servem para determinar como as suas diretivas serão chamadas. Abaixo está uma lista de possibilidades para as restricts:

*   `'A'` - Apenas corresponder por atributo.
*   `'E'` - Apenas corresponder pelo nome do elemento.
*   `'C'` - Apenas corresponder pela classe das CSS.

<div>
<pre class="lang:default decode:true">angular.module('app', [])

.directive('validate', function() {
  return {
    restrict: 'E',
    template: 'Esta é a minha primeira diretiva, e eu ainda estou aprendendo.'
  };
});</pre>
</div>

Mas você também poderá utilizar `AEC` e fazer com que sua diretiva corresponda através de todos esses métodos.

### Melhores práticas

A postagem ainda é breve em relação a este assunto, mas em breve estarei atualizando a postagem, adicionando novas dicas, estarei aumentando a frequência de postagens também.

Um abraço, aguardo vossos comentários, dúvidas, críticas etc.