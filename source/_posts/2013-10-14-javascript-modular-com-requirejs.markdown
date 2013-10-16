---
layout: post
title: "Javascript Modular com RequireJS"
date: 2013-10-14 02:42
comments: true
categories: [javascript, amd, requirejs]
---

O [RequireJS](http://requirejs.org/ "Site oficial do RequireJS") é uma biblioteca para carregar arquivos e módulos javascript que também gerencia as dependências de cada módulo.

Exemplo:

Você tem um módulo de login que depende de um módulo ajax que por sua vez depende de um módulo parseJSON. Ao fazer a requisição do módulo ajax o requirejs vai automáticamente carregar antes o módulo parseJSON e quaisquer dependências que o módulo parseJSON tiver, e então irá carregar o módulo ajax e por fim carregará seu módulo de login.


Por onde começar?
-------------------------

O primeiro passo é baixar o RequireJS no [site oficial](http://requirejs.org/ "Site oficial do RequireJS"), __ou se você usa o [bower](http://bower.io/)__ :


```
bower install --save requirejs
```

O plugin recomenda que você mantenha todo seu código javascript separado do html e então faça apenas a requisição do RequireJs:


```html
<script data-main="scripts/main" src="bower_components/requirejs/require.js"></script>
```

Talvez essa tag script tenha um pouco mais do que você está acostumado, mas é bem fácil entender, no atributo __src__ você vai chamar normalmente o arquivo do Requirejs e no atributo __data-main__ você vai passar o endereço do seu arquivo principal do javascript, aonde você vai carregar os seus módulos (sem a extensão __.js__). Note então que a minha lib que foi instalada pelo bower está na pasta __bower_components/requirejs__ e o meu arquivo principal do javascript está na pasta __scripts__ e se chama __main.js__.

Criando um módulo:
-------------------------

Um módulo é diferente de um arquivo javascript tradicional, é importante entender que cada módulo cria um novo escopo o que evita poluir o namespace global e faz com que um módulo não entre em conflito com outros, um módulo também pode listar suas dependências e trazer elas sem precisar de uma referência a objetos globais e sim receber elas como argumentos na função que define o módulo. Os módulos do RequireJS são extensões do [Javascript Module Pattern](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html), com o benefício de não precisar de variáveis globais.

Definir um módulo é simples:
----------------------------

---

Se o seu módulo não tem nenhuma dependência e possui apenas uma coleção, então você pode passar ele como um objeto literal para a função **_define_** do RequireJs ex.:

```javascript
define({
    user: {
    	name: 'Rafael',
    	age: '24 years'
    }
});
```
---

Caso seu módulo não tenha nenhuma dependência mas é uma função, então você apenas passa uma função como parametro que pode retornar o que você quer tornar público ou a propria função ex.:

```javascript
define(function () {
	function doSomething (arguments) {
		var foo = bar;
	}
	return doSomething;
});
```
---

Caso o seu módulo tenha dependências você precisa apenas passar um array para a função **_define_** com as dependências antes de passar a função ex.:

```javascript
define([
	'lib/ajax'
], function (ajaxModule) {
	function userLogin (user, key) {
		var url = 'http:/login/' + user + '/' + key;
		ajaxModule(url);
	}
	return userLogin;
})

```
No exemplo acima o módulo de ajax está dentro de uma pasta chamada __lib__.

---

#### E a requisição do seu módulo de login ficaria no arquivo **main.js** de alguma das duas formas


Caso o seu módulo não retorne nenhum valor e seja apenas uma função que será auto executada:
```javascript
require(['login', 'otherModule']);

```

Caso o seu módulo esteja retornando algum valor mesmo que este seja a própria função (mais utilizado quando o seu módulo precisa de algum argumento para funcionar):
```javascript
define([
	'modules/login'
], function (userLogin) {
	userLogin('foo', 777);
})

```

Acho que já deu para pegar um pouco do que é o RequireJS e como mudularizar seu código com ele. Em breve farei outros posts mostrando como carregar códigos de terceiros que não são módulos AMD, arquivos de templates, usar o RequireJS para executar seus testes automatizados e otimizar o RequireJS com [RequireJS Optimizer](http://requirejs.org/docs/optimization.html).

