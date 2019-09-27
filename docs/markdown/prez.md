##==##

<!-- .slide:-->

# Sommaire

Back to the core
Wtf is JS?
Closures, Variables & Hoisting
Property Descriptor
This
Asynchronism
Prototypes & Classes
Iterators & Generators
Symbols
Proxies
Modules

Maintainable JavaScript
Typescript
Fn(JS)
Writing Testable code (Avoid Side Effects + Modular code)
Functions as values
map(), filter(), reduce()
HOF & Currying
Write a pipe()
A better Front Architecture -> CQRS !
Let’s write our own Redux Impl°
Bonux: RxJS

##==##

<!-- .slide:-->

Brendan Eich
Inventeur du JavaScript

Un peu d’histoire

![](./assets/images/g584c3d6eeb_0_1.png)

![](./assets/images/masque_photo.png)

Notes:
née en 1961

recruté par Netscape en avril 1995 et prend part au développement du langage
JavaScript
pour le navigateur web
Netscape Navigator

##==##

<!-- .slide:-->

ES2016

ES2015

ES1

ES2018

ES2

ES3

ES5

Juin 1997

Décembre 1999

Juin 2015

Juin 2018

Juin 1998

Décembre 2009

Juin 2016

ES2017

Juin 2017

##==##

<!-- .slide:-->

TC39

Un comité formé de développeurs (actuellement 53 personnes) de divers horizons :
Google
Microsoft
Mozilla
IBM
Airbnb
Universités
Communautés open source
...

https://github.com/orgs/tc39/people

Mais qui conçoit EcmaScript ?

Notes:
née en 1961

recruté par Netscape en avril 1995 et prend part au développement du langage
JavaScript
pour le navigateur web
Netscape Navigator

##==##

<!-- .slide:-->

Stage 0 : strawman
Stage 1 : proposal
Stage 2 : draft
Stage 3 : candidate
Stage 4 : finished

⚠️ les propositions en dessous de stage 4 n’ont aucune garantie d’être incorporées au langage, testez les en projet perso mais pas en prod

http://exploringjs.com/es2016-es2017/ch_tc39-process.html

Les étapes du process TC39

Notes:
née en 1961

recruté par Netscape en avril 1995 et prend part au développement du langage
JavaScript
pour le navigateur web
Netscape Navigator

##==##

<!-- .slide:-->

On peut l’utiliser ?

http://kangax.github.io/compat-table/es6/

http://caniuse.com/#home

![](./assets/images/g584c3d6eeb_0_55.png)

Notes:
http://kangax.github.io/compat-table/es6/

http://caniuse.com/#home

##==##

<!-- .slide:-->

Oui mais je dois être compatible IE 7 ...

Les transpileurs ES.Next -> ES5 (voir ES 3)

![](./assets/images/g584c3d6eeb_0_61.png)

![](./assets/images/g584c3d6eeb_0_62.png)

Notes:
http://kangax.github.io/compat-table/es6/

http://caniuse.com/#home

##==##

<!-- .slide:-->

# Bon, on commence ?

##==##

<!-- .slide: class="with-code" -->

# Comment on procède ?

ES5

```

ES6
ES2015-19...
ESnext


```

Notes:
Object.defineProperty(window, 'CONSTANTE', { value: 1234, enumerable: true, writable: false, configurable: false});

##==##

<!-- .slide:-->

# Closures, Variables & Hoisting

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Const et let : Mais où sont les var ?

```
// On ne peut pas dans un contexte non global

var valueA = 1234;

var valueA = 453; //NO ERROR !!!


```

```
const valueB = 1234;
valueB = 12345; //ERROR

let valueC = 1234;

valueC = 12345;

let valueC = 453; //ERROR



```

Notes:
Object.defineProperty(window, 'CONSTANTE', { value: 1234, enumerable: true, writable: false, configurable: false});

Peut être rajouter une avant dernière ligne :

variable = 453; //NO ERROR

##==##

<!-- .slide: class="with-code"  class="with-code"  class="with-code"  class="with-code" -->

# Const et let : Mais où sont les var ?

```
if (true) {
   var j = 2;}
console.log(j); // 2

```

```
if (true) {
   let j = 2;
   console.log(j); // 2}
console.log(j); // ERROR


```

```
{    function foo () { return 1; }    foo() === 1;    {        function foo () { return 2; }        foo() === 2;    }    foo() === 1;}


```

```
(function () {    var foo = function () { return 1; }    foo() === 1;    (function () {        var foo = function () { return 2; }        foo() === 2;    })();    foo() === 1;})();

```

Notes:
IIFE = Immediately Invoked Function Expression

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Template String

```
var welcome = 'Welcome';var sujet = 'ES6';
var myTemplateHTML = '<div class="sfeirschool">\n<p>'+welcome+'</p>\n<p>'+sujet+'</p>\n</div>';

/*"<div class="sfeirschool"><p>Welcome</p><p>ES6</p></div>"*/





```

```
const welcome = 'Welcome';const sujet = 'ES6';
const myTemplateHTML = `<div class="sfeirschool"><p>${welcome}</p><p>${sujet}</p></div>`;
/*"<div class="sfeirschool"><p>Welcome</p><p>ES6</p></div>"*/


```

On utilise les back quotes `` et on peut créer des expressions avec \${}

##==##

<!-- .slide: class="with-code"  class="with-code"  class="with-code"  class="with-code"  class="with-code"  class="with-code" -->

```
var x = 0, y = 1;
var object = { x: x, y: y };

```

```
let x = 0, y = 1;
const object = { x, y };


```

```
const object = {    foo: 'bar',    [ 'baz' + quux() ]: 42};



```

```
var object = {    foo: 'bar'};object[ 'baz' + quux() ] = 42;


```

# Enhanced Objects

```
const object = {    foo (x, y) {}};



```

```
var object = {    foo: function (x, y) {}};



```

Créer un attribut paramétré

Ajouter une fonction en tant qu’attribut

##==##

<!-- .slide: class="with-code"  class="with-code"  class="with-code"  class="with-code"  class="with-code"  class="with-code" -->

```
function fn (x, y) {    var z = Array.prototype.slice.call(arguments, 2);    return (x + y) + z.length;}fn(1, 2, "hello", true, 7) === 6;


```

```
function fn (x, y, ...z) {    return (x + y) + z.length;}fn(1, 2, "hello", true, 7) === 6;


```

```
const array = [ "Abc", true, 7 ];fn(1, 2, ...array)

```

```
var array = [ "Abc", true, 3 ]; fn.apply(undefined, [ 1, 2 ].concat(array));


```

# Default Params, Rest params, Spread Operator

```
function fn (x = 2, y = 3, z) {    return x + y + z;}


```

```
function fn (x, y, z) {   x || x = 2;   y || y = 3;   return x + y + z;}

```

Spread operator

Rest params

Default params

Notes:
function
fn
(
x
,
y
)

{

if

(
x
===
undefined
) {

# x

2
;

    }

if

(
y
===
undefined
) {

# y

3
;

    }

return
x +
y
;

};

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Fonctions utilitaires - Array

```
var numbers = [ 1, 2, 3 ];
// Vérifier si un tableau inclut une valeurvar hasOne = numbers.indexOf(1) !== -1;
// Remplir un tableau d’une valeur unique
var listOfTens = numbers.map(_ => 10);

var people = [{
  name: “Alice”,
  city: “Nantes”
}, {
  name: “Bob”,
  city: “Bordeaux”
}];

// Trouver la première valeur qui remplit une condition
var alice = people.filter(person => person.name === “Alice”)[0];






```

```
const numbers = [ 1, 2, 3 ];
// Vérifier si un tableau inclut une valeur
const hasOne = numbers.includes(1);// Remplir un tableau d’une valeur unique
const listOfTens = numbers.fill(10);

const people = [{
  name: “Alice”,
  city: “Nantes”
}, {
  name: “Bob”,
  city: “Bordeaux”
}];

// Trouver la première valeur qui remplit une condition
const alice = people.find(person => person.name === “Alice”);


```

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Destructuring, des Arrays

```
var list = [ 1, 2, 3 ];
// Récupérer deux valeursvar a = list[0], b = list[2];
// Echanger les valeursvar tmp = a; a = b; b = tmp;


```

```
const list = [ 1, 2, 3 ];
// Récupérer la première valeur, ignorer la seconde // et prendre la troisièmelet [ a, , b ] = list;
// Echanger les valeurs[ b, a ] = [ a, b ];




```

Notes:
boolean

object

number

null

undefined

symbol

string

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Destructuring, des Objects

```
function getObject () {   return {
     foo: 'foofoo',
     bar: 'barbar',
     quux: 'quuxquux'
   };}

var tmp = getObject();
var foo  = tmp.foo;var bar = tmp.bar;var quux = tmp.quux;
var toto = tmp.toto;



```

```
function getObject () {   return {
     foo: 'foofoo',
     bar: 'barbar',
     quux: 'quuxquux'
   };}

// Variable avec le même nom que l’attribut de l’objetconst { foo, bar, quux, toto } = getObject();

console.log(foo); // foofoo
// Variable avec un nom différent de l’attribut de    // l’objetconst { foo: otherFoo } = getObject();console.log(otherFoo); // foofoo











```

##==##

<!-- .slide: class="with-code"  class="with-code"  class="with-code"  class="with-code" -->

# Destructuring, dans les fonctions

```
function f (arg) {    var name = arg[0];    var val  = arg[1];    console.log(name, val);}

f([ 'bar', 42 ]); // bar, 42


```

```
function f ([ name, val ]) {    console.log(name, val)}

f([ 'bar', 42 ]); // bar, 42


```

```
function f (obj) {    var name = obj.name;    var val  = obj.val;    console.log(name, val);}

f({name: 'bar', val: 42 }); // bar, 42


```

```
function f ({ name, val }) {    console.log(name, val)}

f({name: 'bar', val: 42 }); // bar, 42


```

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Trailing commas

```
var array = [
  "foo",
  "bar", // ok
];var object = {
  a: "foo",
  b: "bar", // ok
};

function(a,b,) { // SyntaxError
  return a + b;
}



```

```
const array = [
  "foo",
  "bar", // toujours ok
];const object = {
  a: "foo",
  b: "bar", // toujours ok
};

function(a,b,) { // maintenant OK (ES2017)
  return a + b;
}



```

Pratique pour éviter des lignes de diff inutiles dans git
Existait déjà pour Array et Object, pas pour les functions
Pas dans les fichiers .json

##==##

<!-- .slide:-->

# Destructuring : Exercice

Modifier la signature de la fonction testDestructuring, de telle sorte qu’elle prenne :

la valeur au second index du tableau qui lui est passé en premier paramètre
l’attribut name du second paramètre
le reste des paramètres dans un tableau

Si aucun objet n’est passé en second paramètre, en fournir un avec comme name “toto”.

##==##

<!-- .slide:-->

# Property Descriptor

& Object enhancements

##==##

<!-- .slide:-->

# Le Property Descriptor

Permet de créer des objets immutables

Chaque propriété d’un objet est pourvu d’un property descriptor

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Le Property Descriptor

```
var sword = { blade: ‘iron’ };Object.getOwnPropertyDescriptor( sword, ‘blade’);// {//   value: ‘iron’,//   writable: true,//   enumerable: true,//   configurable: true// }


```

```
var sword = {};Object.defineProperty( sword, ‘blade’, {  value: ‘steel’,  writable: true,  enumerable: true,  configurable: true});sword.blade; // steel



```

writable : modifier la valeur de la propriété
enumerable : affichage de la propriété lors d’un for...in
configurable : modifier le property descriptor et utiliser l’opérateur delete
(writable: false + configurable: false) permet de créer un objet constant

Notes:
Object.freeze()

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Object.assign

```
const o1 = { a: 1 }, o2 = { b: 2 };Object.defineProperty( o2, "f", {    value: 6,    enumerable: false} );Object.defineProperty( o2, "g", {    value: 8,    enumerable: true} );


```

JS nous permet enfin de copier un objet ou d’en fusionner plusieurs !

```
const newO = Object.assign(target, o1, o2 );

// seuls a, b et Symbol(‘h’) sont copiéstarget.a;                 // 1target.b;                 // 2
target.f;                 // undefined
target.g;                 // 8

newO.a;                 // 1newO.b;                 // 2
newO.f;                 // undefined
newO.g;                 // 8

```

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Fonctions utilitaires sur Object

```
var person = {
  name: "Alice",
  city: "Nantes"
};var keys = [], values = [], entries = [];
for (var key in person) {
   if (Object.prototype.hasOwnProperty.call(person, key))
   {
     keys.push(key);
     values.push(person[key]);
     entries.push([key, person[key]]);
   }
}
console.log(keys) // ["name", "city"]
console.log(values) // ["Alice", "Nantes"]

console.log(entries)
// [["name", "Alice"], ["city", "Nantes"]]




```

```
const person = {
  name: "Alice",
  city: "Nantes"
};const keys = Object.keys(person);
const values = Object.values(person);
const entries = Object.entries(person);

console.log(keys) // ["name", "city"]
console.log(values) // ["Alice", "Nantes"]

console.log(entries)
// [["name", "Alice"], ["city", "Nantes"]]



```

Nouvelles fonctions utilitaires values() et entries(), keys() déjà en ES5

##==##

<!-- .slide:-->

# This

this est souvent mal compris

![](./assets/images/t0ISC9euMpglO.gif)

##==##

<!-- .slide:-->

# This

Ce n’est PAS le this java
Ce n’est PAS une référence au blocking scope

Alors c’est quoi this ?

C’est une référence au contexte du callsite

En résumé this n’est PAS ITSELF

##==##

<!-- .slide:-->

# This

![](./assets/images/3o6gDREoETcSrm9Wog.gif)

##==##

<!-- .slide: class="with-code" -->

# This

```
function counter (iterator) {   console.log(`counter is @ ${iterator}`);   // conserver le nombre d’appel à counter()   this.iterations++;}
counter.iterations = 0;
var iterations = 0;var i;for (i=0; i<5; i++) {   counter(i);}// combien vaut `counter.iterations` ?


```

this.iterations++ ne pointe pas sur la fonction counter

this fait référence au contexte d’exécution (call site lors du runtime)

counter(i) est appelé dans l’objet Global, c’est son call site

sans ‘use strict’ si le call site est l’objet global, alors iterations est une variable globale (i.e, propriété de l’objet globale) qui vaut désormais 5;

avec ‘use strict’ si le call site est l’objet global, alors iterations est NaN.

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# This

```
function counter (iterator) {   console.log(`counter is @ ${iterator}`);   // conserver le nombre d’appel à counter()   this.iterations++;}
function randomFn () {   for (var i = 0; i < 5; i++) {      counter(i);   }}
var iterations = 0;counter.iterations = 0;randomFn.iterations = 0;window.randomFn()// What happened ? ;-)


```

Petit exercice :

Ici lorsque l’on appel randomFn() que se passe-t-il ?

:-)

```
for (var i = 0; i < 5; i++) {      counter.call(randomFn, i)}// randomFn.iterations = 5 ! ENFIN !!!



```

##==##

<!-- .slide: class="with-code"  class="with-code"  class="with-code"  class="with-code"  class="with-code"  class="with-code" -->

# Arrow function

```
var nums = [1, 2, 3, 4, 5, 6];
var odds  = nums.map(function (v) { return v + 1; });






```

```
const nums = [1, 2, 3, 4, 5, 6];
const odds  = nums.map(v => v + 1);



```

```
var nums = [1, 2, 3, 4, 5, 6];
var pairs = nums.map(function (v) { return { even: v, odd: v + 1 }; });





```

```
const nums = [1, 2, 3, 4, 5, 6];
const add  = nums.map((v, _,i) => v + i)
// ou
const add2  = nums.map((v, i) => { return v + i;});



```

```
var nums = [1, 2, 3, 4, 5, 6];
var nums  = nums.map(function (v, i) { return v + i; });






```

```
const nums = [1, 2, 3, 4, 5, 6];
const pairs = nums.map(v => ({ even: v, odd: v + 1 }))


```

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Un problème de this ...

```
   var f = {      nums: [1, 2, 3, 4, 5, 6],      even: [],      each() {        this.nums.forEach(function(v) {          if (v % 2 === 0)            this.even.push(v);//ERROR        });      }   };   f.each();




```

```
//Correction   var f = {      nums: [1, 2, 3, 4, 5, 6],      even: [],      each() {
        var self = this;        this.nums.forEach(function(v) {          if (v % 2 === 0)            self.even.push(v);        });      }   };   f.each();


```

##==##

<!-- .slide: class="with-code" -->

# Résolution avec une Arrow function

```
   var f = {      nums: [1, 2, 3, 4, 5, 6],      even: [],      each() {        this.nums.forEach((v) => {          if (v % 2 === 0)            this.even.push(v);        });      }   };
   f.each(); // [2, 4, 6]	f.even // [2, 4, 6]






```

##==##

<!-- .slide:-->

# Asynchronism

##==##

<!-- .slide:-->

# Promises

C’est une “façon” de gérer les traitements asynchrones avec une api unique en utilisant les callbacks !

.resolve()

.reject()

.then( )

.catch( )

.then( )

.catch( )

.then( )

.catch( )

![](./assets/images/g5875ecc175_2_75.png)

![](./assets/images/g5875ecc175_2_76.png)

![](./assets/images/g5875ecc175_2_80.png)

![](./assets/images/g5875ecc175_2_81.png)

![](./assets/images/g5875ecc175_2_84.png)

![](./assets/images/g5875ecc175_2_85.png)

##==##

<!-- .slide:-->

# Promises

Un petit exemple

.resolve(“ok”) “ok”

.then( fn )

fn(value) { // your code },

value contient “ok”

![](./assets/images/g584c3d6eeb_0_284.png)

![](./assets/images/g584c3d6eeb_0_287.png)

![](./assets/images/g584c3d6eeb_0_290.png)

##==##

<!-- .slide: class="with-code" -->

# Promises

Elles viennent résoudre un problème simple : Aplatir une pile d’appels de callback aussi nommée “Callback Hell”

```
a(function (resultA){  b(resultA, function (resultB){    c(resultB, function (resultC){      d(resultC, function (resultD){        e(resultD, function (resultE){          console.log(resultE);        }      }    }  }}

```

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Promises

Pour cela, on peut chaîner les promesses !

```
a(function (resultA){  b(resultA, function (resultB){    c(resultB, function (resultC){      d(resultC, function (resultD){        e(resultD, function (resultE){          console.log(resultE);        }      }    }  }}

```

```
a()  .then(resultA => b(resultA))  .then(resultB => c(resultB))  .then(resultC => d(resultC))  .then(resultD => e(resultD))  .then(resultE => console.log(resultE))

```

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Promises

Pour cela, on peut chaîner les promesses !

```
function a() {    return new Promise(...);}function b(value) {    return new Promise(...);}etc ...


```

```
a()  .then(resultA => b(resultA))  .then(resultB => c(resultB))  .then(resultC => d(resultC))  .then(resultD => e(resultD))  .then(resultE => console.log(resultE))

```

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Promises

On peut faire encore mieux !

```
a()  .then(resultA => b(resultA))  .then(resultB => c(resultB))  .then(resultC => d(resultC))  .then(resultD => e(resultD))  .then(resultE => console.log(resultE))

```

```
a()  .then(b)  .then(c)  .then(d)  .then(e)  .then(console.log)

```

##==##

<!-- .slide:-->

# Promises

.then( )

.catch( )

...

...

...

https://bevacqua.github.io/promisees

##==##

<!-- .slide:-->

# Promises

On résume

Une promesse renvoie toujours une promesse !

Si une promesse est résolue, la valeur résolue va dans le prochain then()

Si une promesse est rejetée, la valeur rejetée va dans le prochain catch() ( ou dans le then avec la fonction en second paramètre).

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Promises

L’API Promise

```
new Promise(function (resolveFn, rejectFn) {   // Votre code asynchrone});









```

```
Promise.resolve(/* valeur ou une promesse */) // Renvoie une promesse résoluePromise.reject(/* valeur ou une promesse */) // Renvoie une promesse rejetée// ExemplePromise.resolve(4).then(value => console.log(value)) // 4Promise.resolve(Promise.resolve('toto')).then(value => console.log(value)) // 'toto'


```

Créer une nouvelle promesse

Créer une nouvelle promesse à partir d’une valeur ou d’une autre promesse

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Promises

L’API Promise

```
Promise.all([/* tableau de promesses */]) // Renvoie une promesse// ExemplePromise.all([Promise.resolve(4), Promise.resolve('toto')]).then(values => console.log(values));// [4, 'toto']










```

Attendre plusieurs promesses et les fusionner en une

```
Promise.race([/* tableau de promesses */]) // Renvoie une promesse// ExemplePromise.race([Promise.resolve(4), Promise.resolve('toto')]).then(value => console.log(value));// 4










```

Attendre plusieurs promesses, premier arrivé => premier servi

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Promises

```
function asyncFn () {   return new Promise((resolve, reject) => {      var request = new XMLHttpRequest();      request.open('GET', 'http://www.wikipedia.org/', false);       request.onreadystatechange = (event) => {         if (request.readyState == 4) {            if(request.status == 200)               resolve(request.responseText);            else               reject("Erreur pendant le chargement de la page.\n");         }      };   });}
asyncFn()   .then(result => console.log(result))   .catch(error => console.error(error));


```

```
function asyncFn (callback) {      var request = new XMLHttpRequest();      request.open('GET', 'http://www.wikipedia.org/',
false);       request.onreadystatechange = (event) => {         if (request.readyState == 4) {            if(request.status == 200)               callback(null, request.responseText);            else               callback("Erreur pendant le chargement de la page.\n");         }      };}
asyncFn(function callback(err, value) {
	if(err) {
   console.log(err);
}
	else {
	   console.log(value);
	}
});

```

“Ok ok ok, moi je veux créer une promesse, comment je fais ?”

##==##

<!-- .slide:-->

# Promises : Exercice

Dans la fonction exercise1Fn, compléter la création de la promise afin de la résoudre avec la valeur “I love promise” au bout de 500ms. (Utiliser setTimeout). Attacher à cette promesse un then afin de console.log la valeur reçu. Renvoyer la promesse résultante.

Dans la fonction exercise2Fn, compléter la création de la promise afin de la rejeter avec la valeur “I hate rejection” au bout de 500ms. (Utiliser setTimeout). Attacher à cette promesse un catch afin de console.log la valeur reçu. Renvoyer la promesse résultante.

Dans la fonction exercise3Fn, faire une chaîne de promesses en partant de l’objet exercise3 afin d’ajouter 5 à la valeur, de la multiplier par 2 et enfin de soustraire 4. Chacune de ces opérations doivent être réalisées dans un morceau de la chaîne différent. Renvoyer la promesse résultante.

##==##

<!-- .slide:-->

Utiliser des promesses dans un style d’écriture synchrone

# Async/Await

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Async/Await

```
function getData() {  var data;  request('http://whatever/data', function(error, response, body) {    data = body;  });  return data;}

```

```
function main() {  var data = getData();  console.log(data); // undefined}

```

Avant la fonction async, le problème de l’asynchronisme

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Async/Await

```
function logData() {    return getData()    .then(result => {        console.log(result);    });} // ES2015 | On utilise une promesse


```

```
async function logData() {
    const result = await getData();    console.log(result);}

// ES2017 | On utilise async/await


```

Une écriture simplifiée pour une lisibilité accrue

##==##

<!-- .slide:-->

# Event Loop

![](./assets/images/g5875ecc175_2_103.png)

##==##

<!-- .slide:-->

# Petite pause pour vos neurones :)

# #sfeirschool #ESNext #JavaScript

@sfeir @rtorond @sn0rks

![](./assets/images/g584c3d6eeb_0_398.png)

##==##

<!-- .slide:-->

# Les ‘classes’ :-)

##==##

<!-- .slide: class="with-code" -->

# **proto**

**proto** Dunder Proto Permet de manipuler le Prototype interne d’un objet via : Getter + Setter
Prototype est une propriété des constructeurs, **proto** est accessible depuis n’importe quel type de variable.

```
function Aristocat(name) { this.name = name; }
var felix   = new Aristocat('felix');var berlioz = new Aristocat('berlioz');
felix.meow = function() {  console.log('Meow froam felix');}
console.log(felix.__proto__ === Aristocat.prototype);
// true
console.log(felix.__proto__ === berlioz.__proto__);
// true

```

##==##

<!-- .slide: class="with-code" -->

# Classes

```
class SuperCat extends Cat {  constructor(name) {    super(name);    this.superPower = SuperCat.MegaMeow();  }  meow() {    //...    super.meow();  }  static MegaMeow() {    return 'Meeeooooowwww';  }}


```

##==##

<!-- .slide: class="with-code" -->

# Classes

```
class SuperCat extends Cat


```

Vous permet d’étendre une classe

Mot clé class suivi d’un nom

##==##

<!-- .slide: class="with-code" -->

# Classes

```
constructor(name) {    super(name);    this.superPower = SuperCat.MegaMeow();}


```

##==##

<!-- .slide: class="with-code" -->

# Classes

```
meow() {    //...    super.meow();}


```

##==##

<!-- .slide: class="with-code" -->

# Classes

```
static MegaMeow() {    return 'Meeeooooowwww';}


```

Attache une fonction à une classe et non à une instance

##==##

<!-- .slide: class="with-code" -->

# Classes

```
let myCat = new SuperCat('Minet');



```

Une erreur survient si on n’utilise pas le mot clé new

##==##

<!-- .slide:-->

# Classes (mais pas tout à fait)

Attention, cela reste du sucre syntaxique. Nous restons au final dans un modèle prototypal !

![](./assets/images/g584c3d6eeb_0_452.png)

![](./assets/images/g584c3d6eeb_0_453.png)

##==##

<!-- .slide:-->

# Classes (Java) VS Prototype

“Copie des valeurs”

“Référence”

![](./assets/images/g584c3d6eeb_0_458.png)

![](./assets/images/g584c3d6eeb_0_459.png)

##==##

<!-- .slide:-->

# Classes : Exercice

Créer une classe Cat avec un constructeur prenant en paramètre un name. Elle a besoin d’une méthode meow (avec un console.log de votre choix) et d’une méthode play (avec un console.log de votre choix).

Créer une classe MainCoon qui étend la classe Cat avec un constructeur prenant un name et une color. Surcharger la méthode play et appeler le play du parent (avec un console.log de votre choix). Ajouter une méthode sleep (avec un console.log de votre choix).

Tester votre code en ouvrant une console nodejs. Importer votre code dans la console (facile si vous avez tout suivi ;) ) puis construire vos objets.

##==##

<!-- .slide:-->

# Itérateur et générateur

##==##

<!-- .slide:-->

# Itérateur et itérable

“Un itérateur est un objet sachant comment accéder aux éléments d'une collection un par un et qui connaît leur position dans la collection.

En JavaScript, un itérateur expose une méthode next() qui retourne l'élément suivant dans la séquence.

Un objet est considéré comme itérable s'il définit le comportement qu'il aura lors de l'itération

Pour qu'un objet soit itérable, un objet doit implémenter la méthode @@iterator (qui est un Symbol)” MDN

##==##

<!-- .slide: class="with-code" -->

# Itérable : Utilisation de for … of

```
let myArray = [1, 2, 'foo', 3];
for(elem of myArray) {    console.log(elem);}// 1// 2// foo// 3


```

![](./assets/images/g584c3d6eeb_0_483.png)

![](./assets/images/g584c3d6eeb_0_484.png)

##==##

<!-- .slide:-->

# Set & Map

ES6 introduit de nouvelles structures de données empruntées aux langages orientés objet :
Map
Set
WeakMap
WeakSet

Contrairement aux objets, ce sont des itérables et peuvent donc être parcourus par une boucle for...of

Les versions ‘Weak’ offrent un avantage de garbage collection et de prévention des fuites mémoire

##==##

<!-- .slide: class="with-code" -->

```
let data = new Set();data.add("hello").add("goodbye").add("hello");data.size === 2;data.has("hello") === true;for (let key of data) { // insertion order    console.log(key);}
// hello
// goodbyeconst getUniq = array => [...new Set(array)]
getUniq([1, 1, 2]) // [1, 2]




```

# Set & Map

Les Set sont une nouvelle structure comparable aux Arrays

Avantages du Set sur l’array :

Inutile d’utiliser IndexOf

valeurs uniques garanties

Suppression possible sans Slice

Notes:
“Les objets
Set
sont des ensembles de valeurs. Il est possible de les parcourir dans l'ordre d'insertion des éléments.
Une

valeur
d'un élément Set ne peut y apparaître qu'
une seule fois
, il est unique pour cette instance de Set.”
MDN

##==##

<!-- .slide: class="with-code" -->

```
let data = new Map();
let s = {example: "cat"}data.set("hello", 42);data.set(s, 34);data.get(s) === 34;data.size === 2;for (let [ key, val ] of data) {     console.log(`${key} ${val}`);}

// hello 42
// [object Object] 34




```

# Set & Map

Les Map sont une nouvelle structure comparable aux Objets

Avantages de la Map sur l’Objet :

Les clefs peuvent être de tout type

Length est propriété de la Map

Itération dans l’ordre d’insertion des éléments

N’hérite pas du prototype Objets

Notes:
“Un objet
Map
représente une collection de données qui sont des correspondances entre des clés ou valeurs et pour lequel on peut itérer dans l'ordre d'insertion pour lister les différentes clés / valeurs.”
MDN

##==##

<!-- .slide:-->

# Les générateurs

C’est une fonction qui peut se mettre en pause.

##==##

<!-- .slide: class="with-code"  class="with-code"  class="with-code"  class="with-code" -->

# Les générateurs

```
let helloWorld = function*() {   yield 'hello';   yield 'world';}let generatorHello = helloWorld();let value = generatorHello.next();console.log(value)console.log(generatorHello.next()); console.log(generatorHello.next());



```

```
function* ()



```

```
helloWorld()



```

```
generatorHello.next()



```

Permet de créer une fonction générateur

On instancie un générateur

On va à la prochaine étape de notre générateur
=> Le prochain yield

##==##

<!-- .slide: class="with-code" -->

# Les générateurs : fonction par étape

```
let helloWorld = function*() {   console.log('work…');
   yield 'hello';
   console.log('work again …');   yield 'world';}let generatorHello = helloWorld();let value = generatorHello.next();console.log(value)console.log(generatorHello.next()); console.log(generatorHello.next());



```

1er console.log, work...

2eme console.log, work again ...

##==##

<!-- .slide: class="with-code" -->

# Les générateurs

```
let helloWorld = function*() {   yield 'hello';   yield 'world';}let generatorHello = helloWorld();let value = generatorHello.next();console.log(value)console.log(generatorHello.next()); console.log(generatorHello.next());



```

1er console.log, on va jusqu’au premier yield

2nd console.log, on va jusqu’au second yield

3éme console.log, il n’y a plus de yield, notre générateur est terminé.

![](./assets/images/g584c3d6eeb_0_534.png)

##==##

<!-- .slide: class="with-code" -->

```
let helloWorld = function*() {   yield 'hello';
   return 'cut !';   yield 'world';}let generatorHello = helloWorld();let value = generatorHello.next();console.log(value)console.log(generatorHello.next()); console.log(generatorHello.next());



```

# Les générateurs: je peux utiliser return ?

![](./assets/images/g584c3d6eeb_0_546.png)

##==##

<!-- .slide: class="with-code" -->

```
let helloWorld = function*() {   while(1) {
yield 'hello';   }
}
let generatorHello = helloWorld();let value = generatorHello.next();console.log(generatorHello.next()); console.log(generatorHello.next());



```

# Les générateurs infinis ?

##==##

<!-- .slide: class="with-code" -->

# Les générateurs: communication

```
let helloWorld = function*() {   var x = yield 11;   var y = yield x + 1;   yield x + y;}let generatorHello = helloWorld();console.log(generatorHello.next(99))console.log(generatorHello.next(41)); console.log(generatorHello.next(42));console.log(generatorHello.next());



```

Pas encore de yield, cette valeur est ignorée

On passe une valeur au premier yield

![](./assets/images/g584c3d6eeb_0_556.png)

##==##

<!-- .slide: class="with-code" -->

# Les générateurs: c’est des itérateurs !

```
let helloWorld = function*() {   yield 'hello';   yield 'world';}
for(elem of helloWorld()){    console.log(elem)}// hello// world

```

##==##

<!-- .slide:-->

# Les générateurs : Exercice

Créer un générateur permettant de donner les valeurs de la suite de Fibonacci. Pour rappel, f(0) = 0 et f(1) = 1.

Testez votre code dans une console nodejs.

BONUS: Vous pouvez limiter le nombre de ligne avec une bonne utilisation du destructuring.

Notes:
BONUS
: Vous pouvez limiter le nombre de ligne avec une bonne utilisation du destructuring.

##==##

<!-- .slide:-->

# Symbol

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Symbols

```
var key = Symbol("key");typeof key === "symbol"; // true


```

Nouveau type primitif (Donc pas besoin de new)

C’est un type de données unique et inchangeable

```
Symbol("toto") === Symbol("toto"); // false



```

Notes:
Un
symbole
est un type de données unique et inchangeable qui peut être utilisé pour représenter des identifiants pour des propriétés d'un objet. L'objet
Symbol
est un conteneur objet implicite pour le
type de données primitif
symbole.

##==##

<!-- .slide:-->

# Symbols

Liste des symbols proposés

Symbol.match
Symbol.iterator
Symbol.replace
Symbol.search
etc ...

Notes:
Un
symbole
est un type de données unique et inchangeable qui peut être utilisé pour représenter des identifiants pour des propriétés d'un objet. L'objet
Symbol
est un conteneur objet implicite pour le
type de données primitif
symbole.

##==##

<!-- .slide: class="with-code" -->

# Symbols

```
let monItérable = {a: 8, b: 9 , c: 'foo'};monItérable[Symbol.iterator] = function* () {    for(let key in this) {        yield key + ': ' + this[key];    }};[...monItérable] // ["a: 8", "b: 9", "c: foo"]


```

Essayons d’utiliser le symbol iterator

Notes:
Un
symbole
est un type de données unique et inchangeable qui peut être utilisé pour représenter des identifiants pour des propriétés d'un objet. L'objet
Symbol
est un conteneur objet implicite pour le
type de données primitif
symbole.

##==##

<!-- .slide: class="with-code" -->

# Uses Cases des Symbols

```
const mySymbol = Symbol('private');
const iterableObject = {    hello: 'Hello guys',    plop: name => 'yop ${name}',    [mySymbol]() {         console.log('private');    }}for (let x of Object.getOwnPropertyNames(iterableObject)) {    console.log(x);}
// hello
// plop

```

Les propriétés “privées” (tout du moins cachées)

Notes:
Un
symbole
est un type de données unique et inchangeable qui peut être utilisé pour représenter des identifiants pour des propriétés d'un objet. L'objet
Symbol
est un conteneur objet implicite pour le
type de données primitif
symbole.

Symbols are mainly used as unique property keys – a symbol never clashes with any other property key (symbol or string)

Object.getOwnPropertySymbols()

##==##

<!-- .slide: class="with-code" -->

# Uses Cases des Symbols

Créer des constantes qui représentent des concepts

```
const COLOR_RED    = Symbol('Red');const COLOR_YELLOW = Symbol('Yellow');const COLOR_GREEN  = Symbol('Green');const COLOR_PURPLE = Symbol('Purple');function getComplement(color) {    switch (color) {        case COLOR_RED:            return COLOR_GREEN;        case COLOR_PURPLE:            return COLOR_YELLOW;        default:            throw new Exception('Unknown color: '+color);    }}




```

Notes:
Un
symbole
est un type de données unique et inchangeable qui peut être utilisé pour représenter des identifiants pour des propriétés d'un objet. L'objet
Symbol
est un conteneur objet implicite pour le
type de données primitif
symbole.

Symbols are mainly used as unique property keys – a symbol never clashes with any other property key (symbol or string)

##==##

<!-- .slide:-->

# Proxy et protection d’objet

##==##

<!-- .slide:-->

# Le Property Descriptor

Protéger ses objets avant ES6 : Le Property Descriptor

Permet de créer des objets immutables et donc protégés

Chaque propriété d’un objet est pourvu d’un property descriptor

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Le Property Descriptor

```
var sword = { blade: ‘iron’ };Object.getOwnPropertyDescriptor( sword, ‘blade’);// {//   value: ‘iron’,//   writable: true,//   enumerable: true,//   configurable: true// }


```

```
var sword = {};Object.defineProperty( sword, ‘blade’, {  value: ‘steel’,  writable: true,  enumerable: true,  configurable: true});sword.blade; // steel



```

writable : modifier la valeur de la propriété
enumerable : affichage de la propriété lors d’un for...in
configurable : modifier le property descriptor et utiliser l’opérateur delete
(writable: false + configurable: false) permet de créer un objet constant

##==##

<!-- .slide:-->

# Proxies

Un proxy est un “wrapper” d’objet

Il permet d’intercepter, surcharger et/ou de remplacer le comportement d’un objet

Le proxy devient l’interface de votre objet avec lequel vous interagissez, votre objet est donc intouché et protégé

Pas d’outil comparable pre-ES6

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Proxies

```
let p = new Proxy(cible, handler);




```

```
let handler = {    get: function(cible, prop){        return prop in cible ? cible[prop] : 42;    }};let obj = {a: 4};let proxy = new Proxy(obj, handler);proxy.b = 32;console.log(proxy.a, proxy.b); // 4, 32console.log(proxy.c); // 42console.log(obj.c); // undefined

```

L’objet à proxifier

Un objet contenant des “traps” qui sont des fonctions

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Proxies

```
let pilote = { name: 'Le Guennec', jours_de_vol: 50 }; let traps = {  set: function (receiver, property, value) {    if (property === 'jours_de_vol' && value > receiver[property]) {        receiver[property] = value;        return true;    }    console.log('Veuillez insérer une valeur superieur à la précédente');    return false;  }};let proxy_pilote = new Proxy(pilote, traps);


```

Je veux protéger mon objet en écriture

```
pilote;// Object {name: "Le Guennec", jours_de_vol: 50}proxy_pilote;// Proxy {name: "Le Guennec", jours_de_vol: 50}proxy_pilote.jours_de_vol = 40;pilote;// Object {name: "Le Guennec", jours_de_vol: 50}proxy_pilote;// Proxy {name: "Le Guennec", jours_de_vol: 50}proxy_pilote.jours_de_vol = 60;proxy_pilote;// Proxy {name: "Le Guennec", jours_de_vol: 60}pilote;// Object {name: "Le Guennec", jours_de_vol: 60}pilote.jours_de_vol = 30;pilote;// Object {name: "Le Guennec", jours_de_vol: 30}proxy_pilote;// Proxy {name: "Le Guennec", jours_de_vol: 30}


```

##==##

<!-- .slide: class="with-code" -->

# Proxies

Une liste non exhaustive de Handlers

Liste complete : Global Object Proxy
Awesome Use Cases : https://github.com/mikaelbr/awesome-es2015-proxy

```
let traps = {   get() {},      // On accède à une propriété du proxy   set() {},      // On set une propriété sur le proxy   apply() {},    // Le proxy est invoqué en tant que fonction   has() {},      // On test l'existence d’une propriété sur le proxy ou sur sa chaîne prototypale   construct() {} // Le proxy est appelé en tant que constructeur
};


```

##==##

<!-- .slide:-->

# Proxies : Exercice

Un cas d’utilisation
Dans une Array de pseudonymes, on veut que la liste ne contienne que des pseudos uniques et en lowercase.
Par soucis de sécurité, l’accès à la proxy liste ne doit renvoyer aucun caractère spécial (§!^\$%; etc...).
Attention : L’accès à la liste d’origine renvoie malgré tout ces caractères si présents.
A vous de jouer !
Conseil :

console.log('Set prop', prop, value);console.log('Get prop', prop);

##==##

<!-- .slide:-->

# Les modules

##==##

<!-- .slide:-->

# Avant les modules, … la concaténation

![](./assets/images/g584c3d6eeb_0_679.png)

![](./assets/images/g584c3d6eeb_0_680.png)

Notes:
https://addyosmani.com/writing-modular-js/

https://www.jvandemo.com/a-10-minute-primer-to-javascript-modules-module-formats-module-loaders-and-module-bundlers/

##==##

<!-- .slide:-->

# Modules

AMD
(Asynchronous Module Definition)

CommonJS

UMD (Universal Module Definition) (SystemJS)

![](./assets/images/g584c3d6eeb_0_687.png)

![](./assets/images/g584c3d6eeb_0_689.png)

Notes:
https://addyosmani.com/writing-modular-js/

##==##

<!-- .slide:-->

# Modules loader + bundler

![](./assets/images/g584c3d6eeb_0_712.png)

![](./assets/images/g584c3d6eeb_0_713.png)

![](./assets/images/g584c3d6eeb_0_714.png)

![](./assets/images/g584c3d6eeb_0_715.png)

![](./assets/images/g584c3d6eeb_0_716.png)

##==##

<!-- .slide: class="with-code"  class="with-code" -->

# Modules

```
// ------ lib.js ------export const sqrt = Math.sqrt;export function square(x) {    return x * x;}export function diag(x, y) {    return sqrt(square(x) + square(y));}

```

```
// ------ main.js ------import { square, diag } from 'lib';console.log(square(11)); // 121console.log(diag(4, 3)); // 5

```

Export pour exposer ce que l’on souhaite

Import pour récupérer ce que l’on souhaite

##==##

<!-- .slide: class="with-code"  class="with-code"  class="with-code"  class="with-code" -->

# Modules : export default

```
// ------ lib.js ------export function square(x) {    return x * x;}

```

```
// ------ main.js ------import { square } from 'lib';// ouimport * as myLib from 'lib';myLib.square; // function square ...

```

```
// ------ lib.js ------export default function square(x) {    return x * x;}

```

```
// ------ main.js ------import square from 'lib';

```

Notes:
“le bleu, c’est nul à chier”

    		-- Florian

##==##

<!-- .slide:-->

# Modules : On essaye !

Créer un module-C.js qui exporte par défaut une fonction appelé moduleCFn avec à l’intérieur un console.log de votre choix.

Créer un module-B.js qui exporte une fonction appelé moduleBFn avec à l’intérieur un console.log de votre choix et qui exporte la fonction moduleCFn de votre module précédent.

Créer un module-A.js qui importe les deux fonctions précédentes (depuis les deux modules), et utilisez les. Ensuite, utiliser la commande npm run modules et observer le bundle résultant. Vous pouvez vérifier votre travail en glissant le index.html dans votre navigateur.

##==##

<!-- .slide:-->

# Merci & Bon Repos !

# #sfeirschool #ESNext #JavaScript

@sfeir

![](./assets/images/g584c3d6eeb_1_179.png)

##==##

<!-- .slide:-->

# Typescript

# Typage statique en JS

##==##

<!-- .slide:-->

# Why Typescript

Améliorer et sécuriser la production de code JavaScript
Harmoniser la qualité de code dans une équipe
Une façon de tester son code
Le code TypeScript est transcompilé (tsc) en JavaScript

##==##

<!-- .slide:-->

# Typage statique

// Basics
let name: string = 'Dexter';
let age: number = 22;
let isSingle: boolean = true;
// Array
let list: number[] = [1, 2, 3];
let list: Array<number> = [1, 2, 3];
// Tuple
let x: [string, number] = ["hand", 5]
// Enum
enum Color {Blue, White, Red = 5};
let color: Color = Color.red

// Any
const notSure: any = 4;
// Void
function log(): void {
console.log("log");
}
// Undefined / Null
let u: undefined = undefined;
let n: null = null;
let num: number = 5;
num = undefined
// use --strictNullChecks

let someValue: any = “string”;
(<string>someValue).length / (someValue as string).length;

##==##

<!-- .slide:-->

# Interfaces / Function

enum EGender { M, F}

interface IUser {
name: string;
readonly age: number; // ReadonlyArray
gender: EGender;
phone?: string;
};

const userInformation: IUser = {
name: 'Dexter',
age: 22,
gender: EGender.M
}

const getName = (user: IUser): string => user.name;
// ‘Dexter’

const getLastName = (user: IUser): string => user.lastName;
Property 'lastName' does not exist on type 'IUser'.

const getPhone = (user: IUser): string => user.phone;
Type 'string | undefined' is not assignable to type 'string'.

getName({name: ‘Dexter’});
Argument of type '{ name: string; }' is not assignable to parameter of type 'IUser'.
userInformation.age = 10;
// Cannot assign to 'age' because it is a read-only property.

##==##

<!-- .slide:-->

# Interfaces / Function

interface IUserLimited {
name: string;
age: number;
}

interface IgetLimitedUserInformation {
(user: IUser): IUserLimited;
}

const getLimitedUserInformation = (user: IUser): { name: string; age: number } => {
const {name, age} = user;
return { name, age}
}

let getLimitedUserInformation: IgetLimitedUserInformation = function(src, sub) {
const {name, age} = user;
return { name, age}
}

##==##

<!-- .slide:-->

# Generics

function identity(arg: number): number {
return arg;
}
// Bad
function identity(arg: any): any {
return arg;
}

function identity<T>(arg: T): T {
return arg;
}
let output = identity<string>("myString");
let output = identity(42);

Nous ne savons pas quelle sera la forme du type -> generics

##==##

<!-- .slide:-->

# Generics

const user: IUser = { name: 'Dexter' }
const user2: IUser2 = { name: {first: 'Dexter'} }

getIn(user, ['name'])
getIn(user2, ['name', 'first'])

function getIn(obj, arrayOfNestedKeys) {}

Quel est le type de retour de getIn ?

Et dans le cas de plusieurs Key … ?

function getIn<T>(obj: T, arrayOfNestedKeys) {}

function getIn<T, K extend keyof T>(obj: T, arrayOfNestedKeys: [K]): … ? {}

##==##

<!-- .slide:-->

# Overloading

function getIn<T, K extend keyof T>
(obj: T, arrayOfNestedKeys: [K]): T[K];

function getIn<
T,
K1 extend keyof T,
K2 extend keyof T[K1]

> (

    obj: T,
    arrayOfNestedKeys: [K1, K2]

): T[K1][k2];

function getIn<
T,
K1 extend keyof T,
K2 extend keyof T[K1],
K3 extend keyof T[K2]

> (

    obj: T,
    arrayOfNestedKeys: [K1, K2, K3]

): T[K1][k2][K3];

function getIn(obj: any, arrayOfNestedKeys: any[]): any {
// ...
};

Créer plusieurs signatures d’une fonction

##==##

<!-- .slide:-->

# Advanced Type

Intersection -> Combine les types en un
IUser & ILogin

interface IUser {
name: string;
}
interface ILogin {
login: string;
password: string;
}

type TAllUserInfo = IUser & ILogin;

function combineData(user: IUser, login: ILogin): TAllUserInfo {
return {...user, ...login};
}

const user: IUser = {name: 'Brice'};
const login: ILogin = {login: 'log', password: 'pass'};
const result: TAllUserInfo = combineData(user, login);
// Ok

const result: string = combineData(user, login);
// Type 'IUser & ILogin' is not assignable to type 'string'.

##==##

<!-- .slide:-->

# Advanced Type

Union => Peut être un parmis plusieurs type
IFish | IBird

let age: string | number;
age = "12"
age = 12;

function toString(el: string | number): string {
if (typeof el === 'number') {
return el + '';
}
return el;
}
toString(age);
// "12"

interface Bird { fly(); layEggs();}interface Fish { swim(); layEggs();}
function getSmallPet(): Fish | Bird {// ...}
let pet = getSmallPet();

pet.layEggs(); // okay
pet.swim(); // errors

##==##

<!-- .slide:-->

# Advanced Type

Type Guards
parameterName is Type

const isFish =
(pet: Fish | Bird): pet is Fish
=> {
return (<Fish>pet).swim !== undefined;
}

if (isFish(pet)) {
pet.swim();
}
else {
pet.fly();
}

TS sait que c’est un Fish dans le if et déduit un Bird dans le else

const isLoginError = (
login: IloginError | IloginSuccess): login is IloginError => {
return (login as IloginError).ErrorCode !== undefined;
};

if (isLoginError(login)) {
// errror  
} else {
// sucess
}

##==##

<!-- .slide:-->

# Functional JavaScript

##==##

<!-- .slide:-->

Haskell Brooks Curry
Logicien & Mathématicien

Un peu d’histoire

![](./assets/images/g5873c8274a_0_280.png)

![](./assets/images/masque_photo.png)

Notes:
née en 1961

recruté par Netscape en avril 1995 et prend part au développement du langage
JavaScript
pour le navigateur web
Netscape Navigator

##==##

<!-- .slide:-->

# Fn JS : Haskell...

“Principalement connu pour son travail sur la logique combinatoire, ses travaux ont posé les bases de la programmation fonctionnelle” - wikipedia

On lui doit notoirement :
Le Langage Haskell
L’opération de Currying

Et d’une manière Générale
La programmation fonctionnelle

![](./assets/images/g5875ecc175_2_1.png)

##==##

<!-- .slide:-->

# Fn JS : Puis LiveScript...

“In 1995, Netscape Communications recruited Brendan Eich with the goal of embedding the Scheme programming language into its Netscape Navigator.” - wikipedia

Basé sur Scheme, un dialect de Lisp, JS est dès sa conception pensé comme un langage fonctionnel

![](./assets/images/g5875ecc175_2_11.png)

##==##

<!-- .slide:-->

# Fn JS : Function as values

En JavaScript, une fonction est une variable
Elle peut être définit en tant que variable
Elle peut être utilisée en tant qu’argument d’une autre fonction
Elle peut être retournée d’une autre fonction

const doubleSum = (a, b) => (a + b) \* 2;

const doubleSubtraction = (a, b) => (a - b) \* 2;

const sum = (a, b) => a + b;
const subtraction = (a, b) => a - b;

const doubleOperator = (f, a, b) => f(a, b) \* 2;

doubleOperator(sum, 3, 1); // 8
doubleOperator(subtraction, 3, 1); // 4

##==##

<!-- .slide:-->

# Fn JS : Pure Functions

Qu’est-ce qu’une fonction pure ?
Elle retourne un même résultat pour les mêmes arguments. C’est-à-dire : Une fonction pure est Déterministe
Une fonction pure ne produit pas d’effets de bords

var coteA = 10;
var result;
const impureRectangle = (coteB) => {
result = coteA \* coteB;
}
impureRectangle(5); //50

const pureRectangle = (coteA, coteB) => {
return coteA \* coteB;
}
pureRectangle(10, 5); //50

##==##

<!-- .slide:-->

# Fn JS : Pure Functions

let compteur = 1;

function incCompteur(valeur) {
compteur = valeur + 1;
}

incCompteur(compteur);
console.log(compteur); // 2

const compteur = 1;

const incCompteur = (valeur) => valeur + 1;

const nouveauCompteur = incCompteur(compteur);

console.log(compteur); // 1
console.log(nouveauCompteur); // 2

Les avantages des fonctions pures ?
Elles sont beaucoup plus simple à tester ! Il n’y a pas besoin de construire un “contexte” à la fonction (mocks). Simplement à tester son invocation.

##==##

<!-- .slide:-->

# Fn JS : Higher Order Functions

Qu’est-ce qu’une Higher Order Function (HOF) ?
Elle prend N fonction•s en paramètre•s
Elle retourne une fonction

##==##

<!-- .slide:-->

# Fn JS : Higher Order Functions (exemple)

Filter
Prend une collection, retourne une nouvelle collection filtrée.
Si le callback retourne true, la valeur est incluse dans la collection retournée, sinon non.

var numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
var evenNumbers = [];

for (var i = 0; i < numbers.length; i++) {
if (numbers[i] % 2 == 0) {
evenNumbers.push(numbers[i]);
}
}

console.log(evenNumbers); // (6) [0, 2, 4, 6, 8, 10]

const even = n => n % 2 == 0;
const listOfNumbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
listOfNumbers.filter(even); // [0, 2, 4, 6, 8, 10]

##==##

<!-- .slide:-->

# Fn JS : Higher Order Functions (exemple)

var peopleSentences = [];
for (var i = 0; i < people.length; i++) {
var sentence = people[i].name + " is " + people[i].age + " years old";
peopleSentences.push(sentence);
}
console.log(peopleSentences);
// ['TK is 26 years old', 'Kaio is 10 years old', 'Kazumi is 30 years old']

const makeSentence = (person) => `${person.name} is ${person.age} years old`;

const peopleSentences = (people) => people.map(makeSentence);

peopleSentences(people);
// ['TK is 26 years old', 'Kaio is 10 years old', 'Kazumi is 30 years old']

Map
Prend une collection, retourne une nouvelle collection transformée.

var people = [
{ name: "TK", age: 26 },
{ name: "Kaio", age: 10 },
{ name: "Kazumi", age: 30 }
];

##==##

<!-- .slide:-->

# Fn JS : Higher Order Functions (exemple)

var totalAmount = 0;

for (var i = 0; i < orders.length; i++) {
totalAmount += orders[i].amount;
}

console.log(totalAmount); // 120

const sumAmount = (currentTotalAmount, order) => currentTotalAmount + order.amount;

const getTotalAmount = (shoppingCart) => shoppingCart.reduce(sumAmount, 0);

getTotalAmount(orders); // 120

Reduce
Prend une fonction ET une collection, retourne une valeur en combinant ses éléments.

var orders = [
{ productTitle: "Product 1", amount: 10 },
{ productTitle: "Product 2", amount: 30 },
{ productTitle: "Product 3", amount: 20 },
{ productTitle: "Product 4", amount: 60 }
];

##==##

<!-- .slide:-->

# Fn JS : Quiz

Petit Quiz 1
La fonction Array.prototype.some est-elle pure ?

var array = [1, 2, 3, 4, 5];

var even = function(element) {
// checks whether an element is even
return element % 2 === 0;
};

console.log(array.some(even));
// expected output: true

##==##

<!-- .slide:-->

# Fn JS : Quiz

Petit Quiz 2
La fonction Array.prototype.forEach est-elle pure ?

const items = ['item1', 'item2', 'item3'];
const copy = [];

items.forEach(function(item){
copy.push(item);
});

##==##

<!-- .slide:-->

# Fn JS : Quiz

Petit Quiz 3
La fonction Array.prototype.map est-elle pure ?

var numbers = [1, 4, 9];
var doubles = numbers.map(function(num) {
return num \* 2;
});

// doubles is now [2, 8, 18]
// numbers is still [1, 4, 9]

var kvArray = [{key: 1, value: 10},
{key: 2, value: 20},
{key: 3, value: 30}];

var reformattedArray = kvArray.map(obj =>{
var rObj = {};
rObj[obj.key] = obj.value;
return rObj;
});
// reformattedArray is now [{1: 10}, {2: 20}, {3: 30}],

##==##

<!-- .slide:-->

# Fn JS : Exercice

Créer une fonction (getTotalAmount) qui calcul le prix total des livres du panier shoppingCart.

La fonction doit :
filtrer par type “books”
transformer l’objet shoppingCart en collection de montants via map
calculer le montant total via reduce

##==##

<!-- .slide:-->

# Fn JS : Immutability

Immutabilité
Un Objet immutable est un objet qui une fois créé, ne peut pas être modifié

const book = Object.freeze({
title : "How JavaScript Works",
author : "Douglas Crockford"
});
book.title = "Other title";
//Cannot assign to read only property 'title'

##==##

<!-- .slide:-->

# Fn JS : Immutability

Pourquoi ?

Augmenter la Prédictabilité du code et suivre les mutations

Predictability

Les mutations cachent les changements, ce qui crée des “side effects”, et potentiellement des bug. L’immutabilité permet d'accroître la lisibilité d’une architecture et de la simplifier.

Mutation Tracking

L’immutabilité permet d’optimiser l’application en utilisant les références de vos objets en testant les égalités (i.e. la méthode “equals” Java)
Il est plus simple de tester les changements d’état.
Exemple en React, ce pattern nous permet d’éviter les renderings superflus

##==##

<!-- .slide:-->

# Fn JS : Currying

Le Currying, ce n’est pas ça...

![](./assets/images/g5875ecc175_2_20.png)

##==##

<!-- .slide:-->

# Fn JS : Currying

What is Currying?
Currying is a process in functional programming in which we can transform a function with multiple arguments into a sequence of nesting functions. It returns a new function that expects the next argument inline.

It keeps returning a new function until all the arguments are exhausted.

function multiply(a, b, c) {
return a _ b _ c;
}
multiply(1,2,3); // 6

function multiply(a) {
return (b) => {
return (c) => {
return a _ b _ c;
}
}
}
// Ou encore
const multiply = a => b => c => a _ b _ c;
console.log(multiply(1)(2)(3)) // 6

##==##

<!-- .slide:-->

# Fn JS : Currying

What is Currying?
The arguments are kept "alive"(via closure) and all are used in execution when the final function in the currying chain is returned and executed.

Let’s see in details how our function works

const mul1 = multiply(1);
const mul2 = mul1(2);
const result = mul2(3);
log(result); // 6

let mul2 = mul1(2);
// mul2 renvoi
/_
function (c) {
return a _ b _ c
}
}
_/

let mul1 = multiply(1);
/\*

- mul1 renvoi _
  function (b) {
  return (c) => {
  return a _ b _ c
  }
  }
  _/

const result = mul2(3);
// it calculates a _ b _ c with the previously passed in parameters:
a = 1, b = 2, c = 3

// 6

##==##

<!-- .slide:-->

# Fn JS : Currying

What is Currying?
Another practical example

const map = fn => mappable => mappable.map(fn);

const arr = [1, 2, 3, 4, 5, 6];

const doubler = n => n \* 2;
const doubleAll = map(doubler);
const doubled = doubleAll(arr);
log(doubled);
// => [2, 4, 6, 8, 10, 12]

Notes:
const doubled = map(doubler)(arr) ===

arr.map(doubler)

##==##

<!-- .slide:-->

# Fn JS : Currying

What is Currying?

En suivant la même logique que la démonstration précédente,
En prenant le tableau de nombre arr en entrée,
À l’aide de la méthode currifiée “map”, renvoyer un tableau qui donne “Red” sur les chiffres impairs, et “Green” sur les chiffres pairs.

    Vous aurez besoin d’implémenter une méthode isEven;

Notes:
C

##==##

<!-- .slide:-->

# A Better Pattern for JavaScript Applications : CQRS

##==##

<!-- .slide:-->

# Fn JS : CQRS

CQRS : Command query responsibility segregation

Conceptualisé par Bertrand Meyer : Inventeur du langage Eiffel

READ

WRITE

UPDATE

##==##

<!-- .slide:-->

# Fn JS : CQRS

CQRS : Command query responsibility segregation

“It states that every method should either be a command that performs an action, or a query that returns data to the caller, but not both. In other words, Asking a question should not change the answer. More formally, methods should return a value only if they [...] possess no side effects.” - Wikipedia

“CQS is considered by its adherents to have a simplifying effect on a program, making its states (via queries) and state changes (via commands) more comprehensible” - Wikipedia

##==##

<!-- .slide:-->

# Fn JS : CQRS in Front

![](./assets/images/g5873c8274a_0_19.png)

![](./assets/images/g5873c8274a_0_20.png)

![](./assets/images/g5873c8274a_0_21.png)

![](./assets/images/g5873c8274a_0_22.png)

##==##

<!-- .slide:-->

# Fn JS : MVC

![](./assets/images/g5873c8274a_0_27.png)

##==##

<!-- .slide:-->

# Fn JS : Redux

![](./assets/images/g5873c8274a_0_32.png)

##==##

<!-- .slide:-->

# Fn JS : Redux

![](./assets/images/g5875ecc175_2_45.png)

##==##

<!-- .slide:-->

# Fn JS : Redux

Les 3 principes de Redux (issue de la doc)

State is Read-Only
Single source of Truth
Changes are made with pure functions

##==##

<!-- .slide:-->

# Fn JS : Redux - Let’s Code it ourselves !

Codons notre propre implémentation basique de Redux

Pour ça nous allons devoir définir :
Un Store en tant que singleton
Une gestion des Dispatchers
Une gestion des Subscribers

##==##

<!-- .slide:-->

Notre Store

let state;
const getState = () => state;

# Fn JS : Redux - Let’s Code it ourselves !

##==##

<!-- .slide:-->

Notre Dispatcher & le stockage des listeners

const listeners = [];

const dispatch = action => {
store = reducer(action, store);
listeners.forEach(listener => listener())
};

# Fn JS : Redux - Let’s Code it ourselves !

##==##

<!-- .slide:-->

Gérer les Subscribe

const subscribe = (listener) => {
listeners.push(listener);
return () => listeners.filter(lis => lis !== listener)
};

# Fn JS : Redux - Let’s Code it ourselves !

##==##

<!-- .slide:-->

# Fn JS : Redux - Let’s Code it ourselves !

En Synthèse

Avec Redux nous pouvons protéger et stabiliser l’Etat de notre application. Ainsi nous pouvons décrire un contrat d’interface (les Actions) pour spécifier les interactions possibles entre l’utilisateur et l’application.
Les Reducers sont des fonctions pures dans un arbre d’état unique.
Le store est unique et contient l’ensemble de l’état de l’application. Ainsi nous pouvons suivre l’ensemble des évolutions de l’état de l’appli.

##==##

<!-- .slide:-->

# Fn JS : Redux - Let’s Play !

Let’s Play !

![](./assets/images/g5875ecc175_2_64.png)

##==##

<!-- .slide:-->

# Fn JS : Redux - Let’s Play !

Ecrire notre implémentation de Redux
Dispatcher les actions INC et DEC sur les clicks appropriés
en implémentant les méthodes increment() et decrement()
Ajouter le reducer d’ADD_TODO et de l’action DEC

BONUS - Rajouter un delete pour chaque TODO

##==##

<!-- .slide:-->

# Fn JS : Redux - DevTools

Meet your new best friend

Redux DevTools !

![](./assets/images/g5875ecc175_2_63.png)

##==##

<!-- .slide:-->

# Reactive JavaScript

##==##

<!-- .slide:-->

# Rx JS : Observable, Observer, Subscribe

Qu’est-ce qu’un Observable ?
Un Observable représente un flux (stream) sur lequel on va pouvoir opérer.
On peut créer un observable via Rx.Observable.create qui retourne un objet qui contient la méthode subscribe
Subscribe prend un argument, qui est un objet que l’on nomme un Observer
l’Observer est soit :
un objet qui implémente les méthodes next, error et complete
une fonction qui implémente uniquement la fonction next

observable.subscribe(observer)

##==##

<!-- .slide:-->

# Rx JS : Exemple

var myObservable = new Observable(function (observer) {
observer.next(1);
observer.next(2);
observer.next(3);
setTimeout(() => {
observer.next(4);
observer.complete();
}, 1000);
});
console.log('just before subscribe');
myObservable.subscribe({
next: x => console.log('got value ' + x),
error: err => console.error('something wrong occurred: ' + err),
complete: () => console.log('done'),
});
console.log('just after subscribe');

// Just before subscribe

// got value 1

// got value 2

// got value 3

// Just after subscribe

// got value 4

// done

##==##

<!-- .slide:-->

# Rx JS : D’Autres créateurs d’observables

// From event
var clicks = Rx.Observable.fromEvent(document, 'click'); var unsubscribe = clicks.subscribe(x => console.log(x));
unsubscribe()
// MouseEvent object logged to console every time a click
// occurs on the document.

// Converts an array to an Observable
var array = [10, 20, 30];
var result = Rx.Observable.from(array);
result.subscribe(x => console.log(x));
// Results in the following:
// 10 20 30

// Convert the Promise returned by Fetch to an Observable
var result = Rx.Observable.fromPromise(fetch('http://myserver.com/'));

result.subscribe({
next: x => console.log('got value ' + x),
error: err => console.error('something wrong occurred: ' + err),
complete: () => console.log('done'),
});
// Or just next fn
result.subscribe((x) => {
console.log('got value ' + x)
})

##==##

<!-- .slide:-->

# Rx JS : Operators

Un opérateur est une fonction pure qui prend un observable en entrée et génère un observable en sortie
Souscrire à l’observable en sortie souscrit également à l’observable en entrée

function multiplyByTen(input) {
var output = Rx.Observable.create(function subscribe(observer) {
input.subscribe({
next: (v) => observer.next(10 \* v),
error: (err) => observer.error(err),
complete: () => observer.complete()
});
});
return output;
}

var input = Rx.Observable.from([1, 2, 3, 4]);
var output = multiplyByTen(input);
output.subscribe(x => console.log(x));

// 10
// 20
// 30
// 40

##==##

<!-- .slide:-->

# Rx JS : Operators

// 1. Get the maximal value of a series of number
Rx.Observable.of(5, 4, 7, 2, 8)
.max()
.subscribe(x => console.log(x));
// Result In: 8

// 2. Map every click to the clientX position of that click
var clicks = Rx.Observable.fromEvent(document, 'click');
var positions = clicks.map(ev => ev.clientX);
positions.subscribe(x => console.log(x));

// 3. Chaining operators
const input = Rx.Observable.fromEvent(node, 'input')
.map(event => event.target.value)
.filter(value => value.length >= 2)
.subscribe(value => {
// use the `value`
});

##==##

<!-- .slide:-->

# Rx JS : /!\ Attention /!\

Sans souscription, un observable n’est jamais invoqué

De plus, l’invocation de “.subscribe()” est synchrone

Un observable n’est donc pas un eventEmitter, mais plutôt une fonction qui peut renvoyer plusieurs valeurs dans le temps.

##==##

<!-- .slide:-->

# Rx JS : Rx Marbles !

Rx marbles pour Comprendre les Opérateurs : https://rxmarbles.com/

![](./assets/images/g5873c8274a_0_73.png)

##==##

<!-- .slide:-->

# Rx JS : Rx Marbles !

debounceTime() & distinctUntilChanged()

![](./assets/images/g5873c8274a_0_80.png)

![](./assets/images/g5875ecc175_2_68.png)

##==##

<!-- .slide:-->

# Rx JS : Rx Marbles !

take() & pluck()

![](./assets/images/g5873c8274a_0_95.png)

![](./assets/images/g5875ecc175_2_69.png)

##==##

<!-- .slide:-->

# Rx JS : Rx Marbles !

map()

![](./assets/images/g5873c8274a_0_102.png)

##==##

<!-- .slide:-->

# Rx JS : Rx Marbles !

concatMap() vs mergeMap()

![](./assets/images/g5873c8274a_0_130.png)

![](./assets/images/g5875ecc175_2_67.png)

##==##

<!-- .slide:-->

# Rx JS : Rx Marbles !

switchMap()

![](./assets/images/g5873c8274a_0_123.png)

##==##

<!-- .slide:-->

# Rx JS : Exercice

Let’s Pipe All The Functions !

![](./assets/images/g5875ecc175_2_66.png)

##==##

<!-- .slide:-->

Kyle Simpson : You don’t know JS // ES6 & Beyond

ES6Features : Synthèse des features d’ES6 avec exemples d’utilisation

ExploringJS ES2016 & ES2017

http://superherojs.com/

# Pour aller plus loin...

##==##

<!-- .slide:-->

# #sfeirschool #JavaScript

@sfeir

![](./assets/images/g584c3d6eeb_0_812.png)
