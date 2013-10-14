---
layout: post
title: "Javascript Modular com RequireJS"
date: 2013-10-14 02:42
comments: true
categories: [javascript, amd, requirejs]
---

O [RequireJS](http://requirejs.org/) é uma biblioteca para carregar arquivos e módulos javascript que também gerência as dependências de cada módulo.

Exemplo:

Você tem um módulo de login que depende de um módulo ajax que por sua vez depende de um módulo parseJSON. Ao fazer a requisição do módulo ajax o requirejs vai automáticamente carregar antes o módulo parseJSON e qualquer dependências que o módulo parseJSON tiver, e então irá carregar o módulo ajax e por fim carregará seu módulo de login.


Por onde começar?
-------------------------

O primeiro passo é baixar o RequireJS no [site oficial](http://requirejs.org/), <strong>ou</strong> se você usa o [bower](http://bower.io/) :


```
bower install --save requirejs
```

O plugin recomenda que você mantenha todo seu código javascript separado do html e então faça apenas a requisição do requirejs:


```
<script data-main="scripts/main" src="bower_components/requirejs/require.js"></script>
```

Talvez essa tag script tenha um pouco mais do que você está acostumado, mas é bem fácil entender, no atributo <strong>src</strong> você vai chamar normalmente o arquivo do requirejs e no atributo <strong>data-main</strong> você vai passar o endereço do seu arquivo principal do javascript, aonde você vai carregar os seus módulos (sem a extensão <em>.js</em>). Note então que a minha lib que foi instalada pelo bower está na pasta <em>bower_components/requirejs</em> e o meu arquivo principal do javascript está na pasta <em>scripts</em> e se chama <strong>main.js</strong>.

Criando um módulo:
-------------------------

Um módulo é diferente de um arquivo javascript tradicional, é imporatante entender que cada módulo cria um novo escopo o que evita poluir o namespace global e faz com que um módulo não entre em conflito com outros, um módulo também pode listar suas dependências e trazer elas sem precisar de uma referência a objetos globais e sim receber elas como argumentos na função que define o módulo. Os módulos do RequireJS são extensões do [Javascript Module Pattern](http://www.adequatelygood.com/JavaScript-Module-Pattern-In-Depth.html), com o benefício de não precisar de variáveis globais.

Definir um módulo é simples:

> Se o seu módulo não tem nenhuma dependência e é só uma coleção então você pode passar ele como uma objeto literal para a função define o requirejs ex.:

```
define({
    user: {
    	name: 'Rafael',
    	age: '24 years'
    }
});
```

> Se o seu módulo não tem nenhuma dependência mas é uma função, então você apenas passa uma função como parametro que pode retornar o que você quer tornar público ou a propria função ex.:

```
define(function () {
	function doSomething (arguments) {
		var foo = bar;
	}
	return doSomething;
});
```

> E finalmente se o seu módulo tem dependências você precisa apenas passar um array para a função define com as dependências antes de passar a função ex.:

```
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

No caso acima o módulo de ajax está dentro de uma pasta chamada lib.

Acho que já deu para pegar um pouco do que é o RequireJS e como mudularizar seu código com ele, em breve irei fazer um outro post mostrando como carregar códigos de terceiros que não são módulos AMD, arquivos de templates, usar o Require para executar seus testes automatizados e otimizar o RequireJs com [RequireJS Optimizer](http://requirejs.org/docs/optimization.html).

Seu feedback é sempre bem vindo, se tiver algo a falar sobre o post me manda uma mensagem no twitter: [@rafaellyra](http://www.twitter.com/rafaellyra). ou por facebook [rafaellyra](http://www.facebook.com/rafaellyra).