title: 'AngularJS 1.3: Conhecendo os validadores assíncronos'
tags:
  - AngularJS
  - JavaScript
id: 90
categories:
  - AngularJS
  - Frameworks
  - JavaScript
date: 2014-11-02 22:57:13
---

### O que são os $asyncValidators?

Os $asyncValidators, como o nome já diz, são validadores [assíncronos](http://pt.wikipedia.org/wiki/Comunica%C3%A7%C3%A3o_ass%C3%ADncrona), ou seja, eles não são executados pelo Angular de forma independente. Eles representam um dos componentes do [NgModelController](https://docs.angularjs.org/api/ng/type/ngModel.NgModelController) que é o controlador da diretiva mais usada no mundo do AngularJS, o [ngModel](https://docs.angularjs.org/api/ng/directive/ngModel).

### Onde eu deveria utilizar um validador assíncrono?

Você deve utilizar validadores assíncronos junto ao ngModel (obviamente), em um campo de texto em que deverá ser digitado um nome de usuário, um endereço de correio eletrônico etc.

### Como ele funciona?

Dentro de cada validador, deverá ter uma [promessa](https://docs.angularjs.org/api/ng/service/$q), que deverá retornar um resolve ou um reject. Caso retorne um resolved, o campo será definido como válido, caso retorne um reject, o campo será definido como inválido.

Quando o validador for definido como inválido, será adicionada uma nova classe chamada <span class="lang:js decode:true  crayon-inline ">ng-(nome desnormalizado do seu validador)-invalid</span>  ou com o final <span class="lang:js decode:true  crayon-inline">-valid</span> , caso ele seja definido como válido.

Enquanto o seu formulário tiver algum validador pendente, o FormController equivalente receberá a chave <span class="lang:js decode:true  crayon-inline ">$pending</span> , assim como o campo equivalente dentro do [FormController](https://docs.angularjs.org/api/ng/type/form.FormController) (exemplo: <span class="lang:js decode:true  crayon-inline ">form.email.$pending</span> ou <span class="lang:js decode:true  crayon-inline ">form.$pending</span> ), equivalente também receberá.

O campo só será definido como válido ou inválido, quando todos os validadores assíncronos forem resolvidos. Enquanto isso a chave <span class="lang:js decode:true  crayon-inline ">$pending</span>  continuará a existir no campo e no formulário.

Você também pode verificar se o seu campo é válido ou inválido atráves de uma chave dentro da chave $error, que se encontra dentro do FormController atual. Exemplo:

<pre class="lang:js decode:true ">&lt;form name="myForm"&gt;
  &lt;div ng-show="myForm.$pending"&gt;
    Verificando...
  &lt;/div&gt;
  &lt;input ng-model="user.email" unique-email&gt;
  &lt;div ng-show="myForm.email.$pending.uniqueEmail"&gt;
    Verificando...
  &lt;/div&gt;
  &lt;div ng-show="myForm.email.$error.uniqueEmail"&gt;
    Este e-mail já está sendo usado por outra pessoa.
  &lt;/div&gt;
&lt;/form&gt;</pre>

Dessa maneira, você poderá implementar vários validadores assíncronos dentro de um só <span class="lang:js decode:true  crayon-inline ">NgModelController</span> . Veja a próxima seção e entenderá melhor.

### Como utilizar?

Você deve aprender a utilizar a opção require do [$compile](https://docs.angularjs.org/api/ng/service/$compile), que importará o NgModelController da diretiva atual para que você possa inserir o seu validador assíncrono. O seu validador será definido por uma nova chave em NgModelController.$asyncValidators, a chave deverá ser o nome do seu validador.

### Exemplo

<pre class="">.directive('uniqueUsername', function ($http, $q) {
   return {
     require: '?ngModel',
     link: function (scope, element, attrs, ngModel) {
       ngModel.$asyncValidators.uniqueUsername = function(modelValue, viewValue) {
         var value = modelValue || viewValue;

         // Procura por um usuário pelo nome
         return $http.get('/api/users/' + value).then(function resolved() {
           return $q.reject('exists');
         }, function rejected() {
           return true;
         });
       };
     }
  };
});</pre>

Espero que tenham gostado, quaisquer dúvidas, deixem seus comentários.