# Ementa JS

A ementa sugere conhecimentos básicos da linguagem Javascript para que, a partir disto, possamos absorver conceitos um pouco mais avançados sobre a linguagem e sua utilização.

## Livros 

* [You Don't Know JS](https://github.com/getify/You-Dont-Know-JS)
  * [Up & Going](https://github.com/getify/You-Dont-Know-JS/blob/master/up%20&%20going/README.md#you-dont-know-js-up--going)
  * [Scope & Closures](https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20&%20closures/README.md#you-dont-know-js-scope--closures)
  * [this & Object Prototypes](https://github.com/getify/You-Dont-Know-JS/blob/master/this%20&%20object%20prototypes/README.md#you-dont-know-js-this--object-prototypes)
  * [Types & Grammar](https://github.com/getify/You-Dont-Know-JS/blob/master/types%20&%20grammar/README.md#you-dont-know-js-types--grammar)
  * [Async & Performance](https://github.com/getify/You-Dont-Know-JS/blob/master/async%20&%20performance/README.md#you-dont-know-js-async--performance)
  * [ES6 & Beyond](https://github.com/getify/You-Dont-Know-JS/blob/master/es6%20&%20beyond/README.md#you-dont-know-js-es6--beyond)
  

Todos os códigos podem ser testados no [JSFiddle](https://jsfiddle.net/)

* [Interação JS/HTML](#interação-jshtml)
	* ToDo

* [Conceitos JS](#conceitos-js)
  * [Function](#function)
  * [Escopo de variáveis](#escopo-de-variáveis)
  * [Closures](#closures)
  * [Callbacks](#callbacks)
  * [Objetos](#objetos)
  * [this](#this)  
  * [JSON](#json)
  * [Estendendo objetos próprios](#estendendo-objetos-próprios)
  * [Estendendo objetos core do JS](#estendendo-objetos-core-do-js)
  * [AJAX](#ajax)
  * [Promises](#promises)

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
ace
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
    callback(hello); //chama a função de callback
}

helloSomething('Hello', function(msg) {
	console.log(msg + " " +'world'); //irá imprimir 'Hello world'
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
	console.log(msg + " " +'world'); //irá imprimir 'Hello world'
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

#### Objetos
_O tipo mais comum do Javscript é o Object. Javascript possui apenas um tipo complexo, que é o Object e cinco tipos simples, que são Number, String, Boolean, Undefined e Null_

_Tipos simples (primitivos), são imutáveis, enquanto o tipo Object é mutável_
```javascript
var operacao  = new Object();
operacao.a = 2;
operacao.b = 3;

operacao.soma = function () {
	console.log('Soma: ' + (operacao.a+ operacao.b));
}
operacao.subtracao = function () {
	console.log('Soma: ' + (operacao.a-operacao.b));
}

operacao.soma(); //irá imprimir 5
operacao.subtracao(); //irá imprimir -1
```
_Antes de irmos mais a fundo neste tópico, vamos relembrar que funções em Javascript também são objetos_

```javascript
function operacao() {
    function soma(a,b) {
        console.log(a+b);
    }
    
    return soma;
}

var op = operacao();
op(2,3); //irá imprimir 5

```
_Reescrevendo, para podemos criar mais operações_

```javascript
function operacao() {
    var obj = new Object();
    obj.soma = function(a, b)  {
    	return a + b;
    }
    obj.subtracao = function(a, b)  {
    	return a - b;
    }
    return obj;
}

var op = operacao();
console.log(op.soma(2,3)); //irá imprimir '5'
console.log(op.subtracao(2,3)); //irá imprimir '-1'
```

#### this
_Todas as funções em Javscript possuem propriedades (pois são objetos). Quando uma função é executada, esta pega a propriedade this (this é uma variável com com o valueOf do objeto que invocou a função onde o this está sendo utilizado_

```javascript
var op = new Object();
op.a = 3;
op.b = 2;
op.soma = function() {
	return this.a + this.b;
}

console.log(op.soma()); // irá  imprimir '5'
```
_Do mesmo modo, podemos ter_

```javascript
function operador(a, b) {
    this.a = a;
    this.b = b;
    function soma() {
    	return this.a + this.b;
    }
    return soma
}
var op = operador(2,3);
console.log(op()); // irá  imprimir '5'
```
_Relembrando que Cloures tem acesso ao escopo da função exterior_

_Usando construtores:_

```javascript
function Person (name, age) {
	this.name = name;
    this.age = age;
    
    this.getName = function() {
    	return this.name;
    }
    this.getAge = function() {
    	return this.age;
    }
}

var pessoa = new Person('Pedro', 27);

console.log(pessoa.getName()); //irá imprimir Pedro
console.log(pessoa.getAge()); //irá imprimir 27
```

_Agora, considere o seguinte caso:_

```javascript
function Numbers () {
	this.list = [1, 2, 3, 4];
    
    this.getMultiple = function(x) {
    	this.mList = [];
        this.list.forEach(function(number) {
            this.mList.push(number*x); //multiplica  todos os números da lista por x
        });
        return this.mList;
    }
  
}

var numbers = new Numbers();

console.log(numbers.getMultiple(2)); 
```
_Ao rodar este código, irá ocorrer um erro_

_Relembrando o tópico de [Escopo de Variáveis](#escopo-de-variáveis), temos que as variáveis são locais dentro de funções e não dentro de blocos. Assim, o this passou a se referir à função anônima criada para iterar sobre a lista_

_Para resolver este caso, temos:_
```javascript
function Numbers () {
	this.list = [1, 2, 3, 4];
    
    this.getMultiple = function(x) {
    	this.mList = [];
        var myObject = this;
        this.list.forEach(function(number) {
            myObject.mList.push(number*x); //multiplica  todos os números da lista por x
        });
        return this.mList;
    }
  
}

var numbers = new Numbers();

console.log(numbers.getMultiple(2)); //irá imprimir [2, 4, 6, 8]
```

_O retorno da função está com this.list pois, em Javascript, copiamos um objeto como referência (lembra-se de ponteiros?)_

#### JSON

_JSON é um acrônimo para Javascript Object Notation, sendo muito utilizado por ser um formato leve para intercâmbio de dados computacionais. JSON é um subconjunto da notação de objeto de JavaScript, mas seu uso não requer JavaScript exclusivamente. Fonte: [Wikipedia](https://pt.wikipedia.org/wiki/JSON)_

_Em outras palavras, JSON é um objeto em Javascript. Assim como temos a declaração de array como new Array() ou [], temos a declaração de objetos como new Object ou {}_

_JSON é muito utilizado em APIs, por exemplo. Iremos mais uma vez utilizar os operadores, para mostrar como podemos trabalhar com objetos_

```javascript
function Operador ()  {
	return {
    	soma: function(a, b) {
        	return a+b;
        },
        subtracao: function(a, b) {
        	return a-b;
        }
    }
}

var op = new Operador();

console.log(op.soma(2,3)); //5
console.log(op.subtracao(2,3)); //-1
```

_Podemos simplificar_

```javascript
var op = {
    soma: function(a, b) {
        return a+b;
    },
    subtracao: function(a, b) {
        return a-b;
    }
}

console.log(op.soma(2,3)); //5
console.log(op.subtracao(2,3)); //-1
//uma segunda maneira de acessar os métodos
console.log(op['soma'](2,3));
console.log(op['subtracao'](2,3));
```
_Sabendo que podemos acessar os métodos como um array, podemos utilizar variáveis para  condicionar a chamada_

```javascript
var op = {
    soma: function(a, b) {
        return a+b;
    },
    subtracao: function(a, b) {
        return a-b;
    }
}
var nomeOperacao = 'soma';
console.log(op[nomeOperacao](2,3)); //5
```

#### Estendendo objetos próprios
_O Javascript permite que métodos sejam adicionados aos seus objetos, prototipando uma nova funcionalidade, permitindo que todas as instâncias deste objeto utilizem este método_

```javascript
function Message(txt) {
	this.txt = txt;
}

Message.prototype.print = function() {
	console.log(this.txt);
}

var msg = new Message('Oi, eu sou uma mensagem');
msg.print();
```

#### Estendendo objetos core do JS
_Dado que conseguimos prototipar nossos próprios objetos, será que conseguimos prototitpar objetos nativos do JS? Vamos fazer um teste:_

```javascript
function List()
{
    return {
        itens: [],
        bubbleSort: function() {
            var swapped;
            do {
                swapped = false;
                for (var i=0; i < this.itens.length-1; i++) {
                    if (this.itens[i] > this.itens[i+1]) {
                        var temp = this.itens[i];
                        this.itens[i] = this.itens[i+1];
                        this.itens[i+1] = temp;
                        swapped = true;
                    }
                }
            } while (swapped);
        }
    }
}

var myList = new List();
myList.itens = [1, 4, 2, 7, 3, 6, 5]; 
console.log(myList.itens); //[1, 4, 2, 7, 3, 6, 5]
myList.bubbleSort();
console.log(myList.itens); //[1, 2, 3, 4, 5 ,6, 7]
```

_Será que podemos fazer com que todos os arrays tenham o bubbleSort 'nativo'?_

```javascript
Array.prototype.bubbleSort = function() {
    var swapped;
    do {
        swapped = false;
        for (var i=0; i < this.length-1; i++) {
            if (this[i] > this[i+1]) {
                var temp = this[i];
                this[i] = this[i+1];
                this[i+1] = temp;
                swapped = true;
            }
        }
    } while (swapped);
}
```

_Modificamos o código do bubbleSort para apontar para a própria lista. Com isto, podemos fazer:_

```javascript
var list = [1, 4, 2, 7, 3, 6, 5];
console.log(list); //[1, 4, 2, 7, 3, 6, 5]
list.bubbleSort();
console.log(list); //[1, 2, 3, 4, 5 ,6, 7]
```

#### AJAX

_AJAX é um acrônimo para Asynchronous Javascript and XML. Como o nome diz, é o responsável por fazer requisições assíncronas no Javascript, possibilitando uma gama de possibilidades e interatividade com o usuário_

_Diversos plugins e frameworks facilitam a utilização desta tecnologia, mas veremos aqui um pouco do código bruto_

_Existem diversos tipos de requisições que podem ser feitas ao servidor, mas as  mais comuns são GET e POST. Este [link](http://www.w3schools.com/tags/ref_httpmethods.asp) explica suas diferenças e, de quebra, ainda mostra os demais tipos de requisição (e especificações de uso)_

_Para os exemplos assíncronos, irei utilizar o [RandomUser.me](https://randomuser.me/), um site que aceita requisições do tipo GET e POST para retornar JSON de teste_

```javascript
function ajaxCall() {
	var xmlhttp;
    if (window.XMLHttpRequest) {
  		//IE7+, Firefox, Chrome, Opera, Safari
        xmlhttp=new XMLHttpRequest();
  	} else {
    	//IE6, IE5
  		xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  	}
	xmlhttp.onreadystatechange = function() {
        //state 4 significa completado e http 200, OK
  		if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
    		console.log(xmlhttp.responseText); //iremos logar os dados do usuário
    	}
  	}
	xmlhttp.open("GET","https://randomuser.me/api/",true);
	xmlhttp.send();
}

ajaxCall(); //chamada da função
```

_A maioria dos plugins e frameworks JS possuem um método simplificado para criar estas chamadas_

#### Promises
_Promise é um Objeto do JS, responsável pelo tratamento de processos assíncronos e demorados_

_Podemos englobar trechos de códigos complexos, para que eles sejam processados de forma assíncrona, não impactando no fluxo restante da aplicação_

_A Promise possui dois parâmetros: resolve e reject. O primeiro argumento conclui a promise, o segundo argumento rejeita. Nós podemos chamar estas funções, somente após o fluxo de código estar completo_

```javascript
var async = function() {
	//criamos a promisse
    return new Promise(function(resolve, reject) {
    	window.setTimeout(
        	function() {
          	//após 3 segundos, resolvemos a promise
          	resolve();
        }, 3000);
	});
};

async().then(function() {
  console.log('3 segundos se passaram');
});
```
_Podemos passar um parâmetro, tanto para resolve, quanto para reject, para retornarmos algum dado após o processamento assíncrono_

```javascript
var async = function() {
    return new Promise(function(resolve, reject) {
        var asyncTime = 3000;
        window.setTimeout(
        function() {
          // We fulfill the promise !
          resolve(asyncTime)
        }, asyncTime);
  	});
};

async().then(function(data) {
    console.log((data/1000) + ' segundos se passaram');
});
```

_Agora, analise este caso:_

```javascript
var async = function() {
    return new Promise(function(resolve, reject) {
        var asyncTime = 3000;
        window.setTimeout(
        function() {
          resolve(x+2)
        }, asyncTime);
  	});
};

async().then(function(data) {
    console.log('Output: ' + data);
});
```

_Um erro foi retornado no console, pois x não está definido. Como tratar este erro?_

```javascript
var async = function() {
    return new Promise(function(resolve, reject) {
        resolve(x+2);
  	});
};

async().then(function(data) {
    console.log('Output: ' + data);
}).catch(function(error) {
	console.log(error); //irá imprimir o erro do interpretador
});
```
_Podemos personalizar o reject_

```javascript
var async = function() {
    return new Promise(function(resolve, reject) {
        try {
        	resolve(x+2);
        } catch(e) {
        	reject('x não está definido');
        }
  	});
};

async().then(function(data) {
    console.log('Output: ' + data);
}).catch(function(error) {
	console.log(error);
});
```
_O then() aceita dois parâmetros para sua execução: success e error. Ao invés de usarmos o catch, poderíamos escrever da seguinte maneira:_

```javascript
var async = function() {
    return new Promise(function(resolve, reject) {
        try {
        	resolve(x+2);
        } catch(e) {
        	reject('x não está definido');
        }
  	});
};

async().then(function(data) {
    console.log('Output: ' + data);
}, function (error) { 
	console.log(error);
});
```
_Cada vez mais, o objeto Promise vem ganhando visibilidade, devido a sua capacidade de englobar blocos de código assincronamente, evitando assim a pausa no fluxo de execução do restante da aplicação_

_Com isto, podemos transformar uma simples requisição AJAX em uma requisição com maior controle, utilizando Promise_

```javascript
function ajaxCall() {
    return new Promise(function(resolve, reject){
        var xmlhttp;
        if (window.XMLHttpRequest) {
            //IE7+, Firefox, Chrome, Opera, Safari
            xmlhttp=new XMLHttpRequest();
        } else {
            //IE6, IE5
            xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
        }
        xmlhttp.open("GET","https://randomuser.me/api/",true);
        xmlhttp.send();
        xmlhttp.onload = function () {
          if (xmlhttp.status == 200) {
            resolve(xmlhttp.responseText);
          } else {         
            reject(this.statusText);
          }
        };
        xmlhttp.onerror = function () {
          reject(this.statusText);
        };
    });
}

var p = ajaxCall(); //chamada da função

p.then(function(data) {
	console.log(data);
}, function(error){
	console.log(error);
});
```
#### JQuery
TODO


#### AngularJS
TODO

