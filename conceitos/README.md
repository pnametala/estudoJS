# Conceitos Importantes

* [Premissas Básicas](#premissas-básicas)
* [Função Anônima](#função-anônima)
* [Escopo de variáveis](#escopo-de-variáveis)
* [Hoisting](#hoisting)
* [Closures](#closures)
* [Callbacks](#callbacks)
* [Objetos](#objetos)
* [this](#this)  
* [JSON](#json)
* [Estendendo objetos próprios](#estendendo-objetos-próprios)
* [Estendendo objetos core do JS](#estendendo-objetos-core-do-js)
* [Fluent Interface](#fluent-interface)
* [AJAX](#ajax)
* [Promises](#promises)
* [Programação funcional](#programação-funcional)
	* [Programação Imperativa X Programação Funcional](#programação-imperativa-x-programação-funcional)
	* [Memoization](#memoization)
	* [Libs Funcionais](#libs-funcionais)

#### Premissas Básicas
_Antes de entrarmos em plugins e frameworks JS, seria interessante saber o que a linguagem em questão nos proporciona_  

_Para melhor absorção dos conteúdos aqui apresentados, é aconselhável que se tenha um breve conhecimento de programação e, preferencialmente, conhecimento básico/intermediário de Javascript, para que os conceitos aqui vistos possam ser levados adiante_

#### Função Anônima
_Funções anônimas são funcões sem nome. Utilizamos muito as funções anônimas como callback em outras chamadas, para executar um bloco de códigos após determinada ação_

```javascript
(function () {
   console.log('startei');   
});
```
_Uma função, além de anônima, pode ser o que chamamos de self-invoking (ou seja, ela será executada automaticamente). basta adicionar () ao final dela:_

```javascript
(function () {
   console.log('startei');   
})();
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

#### Hoisting

_Em Javascript, Hoisting é uma etapa da interpretação, onde todas as funções declaradas são lidas prioritariamente_

_Por padrão, a etapa de Hoisting move todas as declarações para o topo do escopo atual (script ou função)_

_Na prática, podemos chamar uma função antes mesmo de declará-la_

```javascript
console.log(hoisting());

function hoisting() {
	return 'função declarada depois';
}
```

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
_O tipo mais comum do Javascript é o Object. Javascript possui apenas um tipo complexo, que é o Object e cinco tipos simples, que são Number, String, Boolean, Undefined e Null_

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
_Todas as funções em Javascript possuem propriedades (pois são objetos). Quando uma função é executada, esta pega a propriedade this (this é uma variável com com o valueOf do objeto que invocou a função onde o this está sendo utilizado_

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
_Relembrando que Closures tem acesso ao escopo da função exterior_

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

_O retorno da função está com this.mList pois, em Javascript, copiamos um objeto como referência (lembra-se de ponteiros?)_

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

#### Fluent Interface
_[Fluent Interface](https://en.wikipedia.org/wiki/Fluent_interface) é uma implementação comum às linguagens Orientadas a Objetos. Provavelmente você já utilizou diverssas vezes, mas não sabia seu nome_

_Esta prática consiste em criar códigos mais legíveis, encadeando chamadas de métodos de uma mesma intância_

```javascript
var obj = {
	foo: function () {
    	console.log('foo');
        return this;
    },
    bar: function () {
    	console.log('bar');
        return this;
    },
};

obj.foo()
    .bar(); //irá imprimir 'foo' e 'bar'
```

_Para uma melhor estruturação do código, podemos utilizar o prototype dos objetos para criar estas funções_

```javascript
function Pessoa() {
}

Pessoa.prototype.setNome = function(nome) {
	this.nome = nome;
    return this;
}
Pessoa.prototype.setSobrenome = function(sobrenome) {
	this.sobrenome = sobrenome;
    return this;
}
Pessoa.prototype.getNome = function() {
	console.log(this.nome + " " + this.sobrenome)
    return this;
}

new Pessoa()
    .setNome('Pedro')
    .setSobrenome('Nametala')
    .getNome();
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
            reject(xmlhttp.statusText);
          }
        };
        xmlhttp.onerror = function () {
          reject(xmlhttp.statusText);
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

#### Programação funcional
_[Programação funcional](https://en.wikipedia.org/wiki/Functional_programming) é um paradigma de computação utilizado há muito tempo na Computação científica (estudos e pesquisas), porém pouco difundido fora deste meio_

_Este consiste em considerar função como expressões matemáticas e evitar mudanças de estado e dados mutáveis_

_Considere a seguinte expressão matemática:_

```Math
y = x + 2
```
_Após assimilarmos este paradigma, nos apresentam a mesma função de outra maneira:_

```Math
f(x) = x + 2
```

_Assim, nos introduzem o conceito e função, variáveis relativas (y/f(x) é relativo a x)_

_A primeira maneira de validaramos isto, é montando a tabela de valores:_


| x | f(x) = x + 2  |   f(x)   |
|:-:|:-------------:|:--------:|
| 0 |  f(0) = 0 + 2 | f(0) = 2 |
| 1 |  f(1) = 1 + 2 | f(1) = 3 |
| 2 |  f(2) = 2 + 2 | f(2) = 4 |
| 3 |  f(3) = 3 + 2 | f(3) = 5 |

_Como transformar isto numa função?_

```javascript
function f(x) {
	return x + 2;
}

console.log(f(0));
console.log(f(1));
console.log(f(2));
console.log(f(3));
```

##### Programação Imperativa X Programação Funcional

_A programação clássica é imperativa. Consiste em verificação  de estados e iterações sobre elementos, desviando fluxos e usando condicionais para tomadas de decisões_

_A programação funcional evita ao máximo o uso dessas técnicas, escrevendo um código mais elegante e otimizado_

_No Javascript, como auxílio de First-class functions, funções anônimas e closures, escrever funcionalmente se torna um pouco menos complicado_

_Vamos escrever uma função para calcular a sequencia Fibonacci_:

```javascript
var fibonacci = function(n) {
    var a = 0, b = 1, f = 1;
    for(var i = 2; i <= n; i++) {
        f = a + b;
        a = b;
        b = f;
    }
    return f;
};

console.log(fibonacci(6)); //8
```

_Muitas vezes, aprendemos a escrever este mesmo algoritmo, através do conceito de [Recursividade](https://en.wikipedia.org/wiki/Recursion_(computer_science))_

```javascript
var recursiveFibonacci = function(n) {
    if(n <= 2) {
        return 1;
    } else {
        return recursiveFibonacci(n - 1) + recursiveFibonacci(n - 2);
    }
};

console.log(recursiveFibonacci(6)); //8
```

_Chamadas recursivas consistem em utilizar a própria função novamente, para efetuar o cálculo. Ao invés de usarmos condicionais e loops para chegar ao nosso resultado, condicionamos o cálculo a uma expressão funcional (ou seja, já  utilizamos a programação funcional no nosso dia  a dia, muitas vezes sem saber)_

_Vamos reescrever f(x) = x + 2 de forma recursiva:_

```javascript
var recursiveF = function(n) {
	if(n <= 0) {
    	return 2;
    }
    return recursiveF(n-1) + 1;
}

console.log(recursiveF(0)); //2
console.log(recursiveF(1)); //3
console.log(recursiveF(2)); //4
console.log(recursiveF(3)); //5
```

##### Memoization

_Memoization é uma técnica computacional de otimização, que consiste em cachear informações de resultados (complexos ou de acesso constante), para alcançar uma performance maior nos algoritmos_

_Reescrevendo a função f(x) = x + 2, utilizando recursividade e memoization, chegamos ao seguinte algoritmo_

```javascript
var recursiveF = function(n) {
	if(n <= 0) {
    	return recursiveF.memoize[n] = 2;
    }
    return recursiveF.memoize[n] = recursiveF.memoize[n - 1] + 1 || recursiveF(n-1);  
}
recursiveF.memoize = {};

console.log(recursiveF(0)); //2
console.log(recursiveF(1)); //3
console.log(recursiveF(2)); //4
console.log(recursiveF(3)); //5
```

##### Libs Funcionais

_Exitem diversas libs js para o auxílio na criação de estruturas funcionais. As duas mais conhecidas são [Underscore.js](http://underscorejs.org/) e [Lodash](https://lodash.com/)_

_Este exemplo, unsando UnderscoreJS, pode ser rodado neste [Fiddle](https://jsfiddle.net/vys01LkL/)_

```javascript
var list = [1 , 2, 3, 4, 5];


function multiple(n) { 
	function get(x) {
    	return x*n;
    }
    return get;
};

var mul = multiple(2); //primeiro argumento passado, fazendo a multiplicação dos itens ser por 2

console.log(_.map(list,mul)); //função map do underscore, que percorerá lista, e chamará uma função para ser executada para cada elemento, imprimindo [2, 4, 6, 8, 10]
```


