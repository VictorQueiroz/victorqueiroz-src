title: 'Jasmine: Primeiro contato com testes unitários'
tags:
  - Jasmine
  - JavaScript
  - Teste de unidade
  - Testes unitários
id: 47
categories:
  - JavaScript
  - Testes unitários
date: 2014-09-17 21:57:48
---

Já se foram os tempos em que nós desenvolvedores testávamos as nossas aplicações na mão, íamos até um navegador, abrir de página por página e saíamos dando <span class="lang:js decode:true  crayon-inline">console.log</span> em tudo para sabermos se aquilo estava realmente retornando o que esperávamos. E para os que ainda não conhecem, eu trouxe até vocês a apresentação de um ótimo testador unitário que vai mudar o seu jeito de desenvolver as suas aplicações. Hahahahaha! Vamos lá! :D

## Introdução

### O que é Jasmine?

Jasmine é um framework de [desenvolvimento orientado a comportamento](http://pt.wikipedia.org/wiki/Behavior_Driven_Development) para testes de códigos em JavaScript. Não depende de nenhum outro framework de JavaScript. E o melhor de tudo é que ele possuí uma sintaxe limpa e óbvia, então você poderá escrever testes que sejam limpos e fáceis de entender por qualquer outro desenvolvedor.

## Let's type some code

### O que é uma suíte de testes?

Uma suíte de testes é nada mais nada menos que **uma suíte** em que você colocará todos os testes pertencentes a essa suíte dentro dela. É mais estético do que funcional, digamos assim. Exemplo: Eu tenho a suíte de testes chamada 'application module' e dentro dela eu tenho vários testes que irão testar os meu controladores, e outras funcionalidades do módulo <span class="lang:js decode:true  crayon-inline ">application</span> .

### Algumas das funções que o Jasmine me disponibiliza executar

#### **describe (describeName, function)**

Cria uma nova suíte de testes.

#### **it (testDescription, function)**

Cria um novo teste. É dentro dos **it's** em que os testes realmente acontecem.

##### Padrões

É importante que você mantenha um padrão ao determinar o **testDescription**, para isso você vai precisar de um pequeno conhecimento no idioma inglês.

A função <span class="lang:js decode:true  crayon-inline ">it</span> se chama assim por um motivo bem simples, ela serve para que você possa determinar um teste de maneira mais limpa e próxima ao idioma, para que qualquer um que leia consiga entender bem o que você escreveu, ou seja, quando eu for determinar um novo teste eu não vou determiná-lo assim <span class="lang:js decode:true  crayon-inline">it('application module: version test', fn)</span> , eu vou escrever o meu **testDescription **da seguinte maneira: <span class="lang:js decode:true  crayon-inline">it('should have a version', fn)</span> , ou seja, eu literalmente completei o que a função 'começou falando': '**it should have a version**', no português '**isto deve ter uma versão**'. Deu pra entender bem como deve ser este padrão?

##### Notas

> Um **it**, não pode ser chamado fora de uma suíte de testes.
Exemplo:
<pre class="lang:js decode:true">describe('application module', function () {
  it('should have a version', function () {
    // tests should happen here
  });
});</pre>
**ERRADO:**
<pre class="lang:js decode:true">describe('application module', function () {
  // ???
});

it('should have a version', function () {
  // tests should happen here
});</pre>

#### **beforeEach (function)**

Esta função será resolvida antes de cada teste dentro de uma suíte começar a ser executado.

#### **afterEach (function)**

Esta função será resolvida depois de cada teste dentro de uma suíte terminar de ser executado.

#### **expect (value)**

Deve estar sempre dentro de um teste, ela é a última parte de um teste, esta função serve para que você possa verificar se **value **é igual ao valor esperado, é de fato o que chamamos de **teste**. Em **value **exatamente como o próprio parâmetro sugere, deve conter um valor, seja lá qual ele for, a partir desta função, será retornada uma outra função e você poderá utilizá-la de várias formas para finalizar o teste.

*   toBe (value) - Verifica se **value **(definido com o **expect**) é igual a **value **(definido com toBe)
*   toEqual (value) - Verifica se **value **(definido com o **expect**) é igual a **value **(definido com toEqual)
*   toBeDefined() - Verifica se **value **foi definido.
*   toMatch (match) - Executa uma expressão regular em **value **e retorna **true **caso encontre algo.
*   toBeUndefined() - Verifica se **value **não foi definido.
Todas as funções acima podem ser usadas logo após um **.not **o que fará com que elas operem de maneira contrária. Exemplo:
<pre class="lang:js decode:true">describe('application module', function () {
  it('should do something', function () {
    expect(true).not.toBe(false);
    expect(true).toBe(true);
    expect(true).not.toBeUndefined();
  });
});</pre>
Bom, espero que tenham gostado. Até a próxima, e lembrem-se. SIGAM OS PADRÕES e deixem suas dúvidas abaixo, tentarei ao máximo ajudar a todos, um abraço! :)