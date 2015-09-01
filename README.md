# Ementa JS

A ementa sugere conhecimentos básicos da linguagem Javascript para que, a partir disto, possamos absorver conceitos um pouco mais avançados sobre a linguagem e sua utilização.

* [Interação JS/HTML](#interacao-jshtml)
	* ToDo

* [Conceitos JS](#conceitos-js)
  * [Function](#function)
  * [Escopo de variáveis](#escopo-de-variaveis)
  * [Closures](#closures)
  * [Callbacks](#callbacks)
  * [Object](#object)
  * [this](#this)  
  * [JSON](#json)
  * [Estendendo objetos próprios](#estendendo-objetos-proprios)
  * [Estendendo objetos core JS](#estendendo-objetos-core-js)

* [JQuery](#jquery)
	* ToDo
* [AngularJS](#angularjs)
	* ToDo


#### Interação JS/HTML
TODO

#### Conceitos JS
_Antes de entrarmos em plugins e frameworks JS, seria interessante saber o que a linguagem em questão nos proporciona_  

_Neste tópico, iremos demonstrar alguns conceitos desejáveis para o melhor entendimento dos frameworks JS_

#### Function
_Declaração de uma função em Javascript_

```javascript
function nomeDaFuncao(parametros) {
	//todo
}
```
_Função soma em Javascript_

```javascript
function soma(a, b) {
	return a + b;
}
console.log(soma(1,2)) //output = 3
```

#### Escopo de Variáveis
_O escopo de uma variável é o contexto no qual a variável existe. O escopo define de onde onde você pode acessar esta variável e se você tem a acesso a mesma._

_Na maioria das linguagens, o escopo das variáveis é block-level. No JS, o escopo é function-level. Variáveis criadas dentro de uma função são consideradas variáveis locais._

_Function-level scope_

```javascript
var scopeLevel = 'global';
function getScopeLevel() {
	var scopeLevel = 'local';
    console.log(scopeLevel) //irá imprimir 'local'
}
console.log(scopeLevel) //irá imprimir 'global'
```

_No block-level scope_

```javascript
var scopeLevel = 'global';

if(scopeLevel) {
	scopeLevel = 'local';
    console.log(scopeLevel) //irá imprimir 'local'
}

console.log(scopeLevel) //irá imprimir 'local'
```

_Neste segundo exemplo, podemos ver que a variável modificada dentro do if, se mantém para o resto da execução_

_Podemos assim, forçar escopos isolados, utilizando funções anônimas_

```javascript
(function() { 
      var txt = '';

      function funcaoIsolada() {
         console.log('Hello ' + txt);
      }

      txt = 'World';
      funcaoIsolada(); //irá imprimir 'Hello World'

      alert(typeof txt);            //string
      alert(typeof funcaoIsolada);   //function
   })();

   alert(typeof txt);               //undefined
   alert(typeof funcaoIsolada);      //undefined
```
_Variáveis locais possuem prioridade sobre variáveis globais_

_Caso uma variável seja incializada sem declaração, ela será considerada uma variável global_

#### Closures
_Closure é uma função interna, que tem acesso às variáveis externas de sua função exterior_

_Em resumo, uma Closure tem acesso às suas variáveis locais, às variáveis da função que a engloba e acesso às variáveis globais_

_Mas afinal, como funciona a Closure?_

```javascript
function outerFunction(param1, param2) {
	var txt = 'Testando';
    function innerFunction() {
        console.log(txt + " " + param1 + " " + param2); //também tem acesso aos parâmetros da função
    }
    
    return innerFunction();
}

outerFunction('parâmetros', 'closure'); //irá  imprimir 'Testando parâmetros closure'

```
_Uma das principais propriedades das Closures, é que a inner function continua acessível, mesmo após o retorno de sua outer function_

```javascript

function outerFunction(param1) {
	var txt = 'Testando';
    function innerFunction(param2) {
        return txt + " " + param1 + " " + param2; //modificamos que esta função possa retornar 
    }
    
    return innerFunction;
}

var closure = outerFunction('parâmetros');

console.log(closure('closure')); //irá  imprimir 'Testando parâmetros closure'

```

_E com isto, estamos dando o primeiro passo para o estudo de [programação funcional](https://en.wikipedia.org/wiki/Functional_programming)_

#### Callbacks
_Em Javascript, funções são objetos de primeira classe, ou seja, funções do tipo objeto e que podem ser utilizadas de um modo de primeira classe, como qualquer outro objeto (strings, arrays etc), podendo assim, serem armazenados em variáveis e passados como argumentos para funções, declaradas dentro das mesmas e até mesmo retornadas (como visto em Closures)_

_Tendo isto em mente, podemos passar uma função como parâmetro para outra e, de acordo  com nossa lógica, executá-la em algum trecho do código_

```javascript
function helloSomething(hello, callback) {
    callback (hello); //chama a função de callback
}

helloSomething('Hello', function(msg) {
	console.log(msg + " " +'world');
});
```
_Como uma boa prática de programação, é sempre bom garantir que a função de callback seja realmente uma função_

```javascript
function helloSomething(hello, callback) {
	if(typeof callback === "function") {
    	callback(hello); //chama a função de callback
    }
}
	
helloSomething('Hello', function(msg) {
	console.log(msg + " " +'world');
});
```
_Podemos criar callbacks distintos para diversos tratamentos_

```javascript
var a = 3;
var b = 2;

var success  = function (output) {
	console.log('A soma é ' + output);
};
var error  = function (output) {
	console.log('Erro: ' + output);
};

function soma(a, b, success, error) {
    var c = a + b;
    
    if(isNaN(c)) {
    	if(typeof error === "function") {
       		return error('Não foi possível obter a soma'); //chama a função de erro
    	}
    }
    if(typeof success === "function") {
        return success(c); //chama a função de sucesso
    }    
}

soma(a, b, success, error); //irá  imprimir 'A soma é 5'

soma(a, 'teste', success, error); //irá  imprimir 'Erro: Não foi possível obter a soma'
```
_Funções de Callback também são derivadas do paradigma da [programação funcional](https://en.wikipedia.org/wiki/Functional_programming)_
