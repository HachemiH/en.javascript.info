
# Type symbole

Par spécification, les clés de propriété d'objet peuvent être de type chaîne de caractères ou de type symbole. Pas des nombres, pas des booléens, uniquement des chaînes de caractères ou des symboles.

Jusqu'à présent, nous n'avons vu que des chaînes de caractères. Voyons maintenant les avantages que les symboles peuvent nous apporter.

## Symboles

Un “Symbol” représente un identifiant unique.

Une valeur de ce type peut être créée en utilisant `Symbol()` :

```js
// id est un nouveau symbole
let id = Symbol();
```

Lors de la création, nous pouvons donner à symbole une description (également appelée nom de symbole), particulièrement utile pour le débogage :

```js
// id est un symbole avec la description "id"
let id = Symbol("id");
```

Les symboles sont garantis d'être uniques. Même si nous créons beaucoup de symboles avec la même description, ce sont des valeurs différentes. La description est juste une étiquette qui n’affecte rien.

Par exemple, voici deux symboles avec la même description -- ils ne sont pas égaux :

```js run
let id1 = Symbol("id");
let id2 = Symbol("id");

*!*
alert(id1 == id2); // false
*/!*
```

Si vous connaissez Ruby ou un autre langage qui comporte également une sorte de "symboles", attention à ne pas vous tromper. Les symboles JavaScript sont différents.

````warn header="Les symboles ne se convertissent pas automatiquement en chaîne de caractères"
La plupart des valeurs de JavaScript prennent en charge la conversion implicite en chaîne de caractères. Par exemple, nous pouvons `alert` presque toutes les valeurs et cela fonctionnera. Les symboles sont spéciaux. Ils ne se convertissent pas automatiquement.

Par exemple, cette `alert` affichera une erreur :

```js run
let id = Symbol("id");
*!*
alert(id); // TypeError: Impossible de convertir une valeur de symbole en chaîne de caractères
*/!*
```
C'est un "garde du langage" contre les erreurs, parce que les chaînes de caractères et les symboles sont fondamentalement différents et ne doivent accidentellement pas être convertis les uns en les autres. 

Si nous voulons vraiment afficher un symbole, nous devons appeler `.toString()` dessus, comme ici :
```js run
let id = Symbol("id");
*!*
alert(id.toString()); // Symbol(id), maintenant ça marche
*/!*
```
Ou récupérer la propriété  `symbol.description` pour afficher la description uniquement :
```js run
let id = Symbol("id");
*!*
alert(id.description); // id
*/!*
```

````

## Propriétés "cachées"

Les symboles nous permettent de créer des propriétés "cachées" d'un objet, qu'aucune autre partie du code ne peut accéder accidentellement ou écraser.

Par exemple, si nous travaillons avec des objets `user` qui appartiennent à un code tiers, nous aimerions leur ajouter des identificateurs.

Utilisons une clé symbole pour cela :

```js run
let user = { // belongs to another code
  name: "John"
};

let id = Symbol("id");

user[id] = 1;

alert( user[id] ); // nous pouvons accéder aux données en utilisant le symbole comme clé
```

Quel est l’avantage de l’utilisation de `Symbol("id")` sur une chaîne de caractères `"id"` ?

Poussons un peu plus loin l’exemple pour voir cela.

Comme les objets `user` appartiennent à un autre code et que ce code fonctionne également avec eux, nous ne devrions pas simplement y ajouter de champs. C’est dangereux. Cependant, il est parfois impossible d’accéder à un symbole. Mais un symbole ne peut pas être accédé accidentellement, le code tiers ne le verra probablement même pas, il est donc probablement correct de le faire.

Imaginez qu'un autre script veuille avoir son propre identifiant à l'intérieur de `user`, pour sa propre utilisation. Cela peut être une autre bibliothèque JavaScript, donc les scripts ne sont absolument pas conscients les uns des autres.

Ensuite, ce script peut créer son propre `symbol("id")`, comme ceci :

```js
// ...
let id = Symbol("id");

user[id] = "Their id value";
```

Il n'y aura pas de conflit entre nos identificateurs et les leurs, car les symboles sont toujours différents, même s'ils portent le même nom.

Notez que si nous utilisions une chaîne de caractère `"id"` au lieu d'un symbole dans le même but, il y *aurait* un conflit :


```js
let user = { name: "John" };

// Notre script utilise la propriété "id"
user.id = "Our id value";

// ...Un autre script veut aussi "id" pour ses besoins …

user.id = "Their id value"
// Boom! écrasé par un autre script!
```

### Symboles dans un objet littéral

Si nous voulons utiliser un symbole dans un objet littéral `{...}`, nous avons besoin de crochets.

Comme ceci :

```js
let id = Symbol("id");

let user = {
  name: "John",
*!*
  [id]: 123 // pas "id": 123
*/!*
};
```
C’est parce que nous avons besoin de la valeur de la variable `id` comme clé, pas de la chaîne de caractères "id".

### Les symboles sont ignorés par for…in

Les propriétés symboliques ne participent pas à la boucle `for..in`.

Par exemple :

```js run
let id = Symbol("id");
let user = {
  name: "John",
  age: 30,
  [id]: 123
};

*!*
for (let key in user) alert(key); // name, age (pas de symboles)
*/!*

// l'accès direct par le symbole fonctionne
alert( "Direct: " + user[id] );
```

<<<<<<< HEAD
`Object.keys(user)` les ignore également. Cela fait partie du principe général du "dissimulation des propriétés symboliques". Si un autre script ou une bibliothèque parcourt notre objet, il n’accédera pas de manière inattendue à une propriété symbolique.
=======
[Object.keys(user)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys) also ignores them. That's a part of the general "hiding symbolic properties" principle. If another script or a library loops over our object, it won't unexpectedly access a symbolic property.
>>>>>>> 3c934b5a46a76861255e3a4f29da6fd54ab05c8c

En revanche, [Object.assign](mdn:js/Object/assign) copie les propriétés de chaîne de caractères et de symbole :

```js run
let id = Symbol("id");
let user = {
  [id]: 123
};

let clone = Object.assign({}, user);

alert( clone[id] ); // 123
```

Il n’y a pas de paradoxe ici. C'est par conception. L'idée est que lorsque nous clonons un objet ou que nous fusionnons des objets, nous souhaitons généralement que *toutes* les propriétés soient copiées (y compris les symboles tels que `id`).

## Symboles globaux

Comme nous l’avons vu, habituellement tous les symboles sont différents, même s’ils portent les mêmes noms. Mais parfois, nous voulons que les symboles portant le même nom soient les mêmes entités. Par exemple, différentes parties de notre application veulent accéder au symbole `"id"` qui signifie exactement la même propriété.

Pour cela, il existe un *registre de symboles global*. Nous pouvons créer des symboles et y accéder ultérieurement, ce qui garantit que les accès répétés portant le même nom renvoient exactement le même symbole.

Pour lire (créer en cas d'absence) un symbole du registre, utilisez `Symbol.for(key)`.

Cet appel vérifie le registre global et, s’il existe un symbole décrit comme `key`, le renvoie, sinon il crée un nouveau symbole `Symbol(key)` et le stocke dans le registre avec la `key` donnée.

Par exemple :

```js run
// lit le registre global
let id = Symbol.for("id"); // si le symbole n'existait pas, il est créé

// relit le registre (peut-être à partir d'une autre partie du code)
let idAgain = Symbol.for("id");

// le même symbole
alert( id === idAgain ); // true
```

Les symboles à l'intérieur de ce registre sont appelés *symboles globaux*. Si nous voulons un symbole à l’échelle de l’application, accessible partout dans le code, c’est ce moyen que nous allons utiliser.

```smart header="Cela ressemble à Ruby"
Dans certains langages de programmation, comme Ruby, il existe un seul symbole par nom.

Comme nous pouvons le constater, en JavaScript, c’est bien pour les symboles globaux.
```

### Symbol.keyFor

Pour les symboles globaux, pas seulement `Symbol.for(key)` renvoie un symbole par son nom, mais il existe un appel inversé : `Symbol.keyFor(sym)`, cela fait l'inverse: retourne un nom par un symbole global.

Par exemple :

```js run
// get symbol by name
let sym = Symbol.for("name");
let sym2 = Symbol.for("id");

// obtenir le nom par symbole
alert( Symbol.keyFor(sym) ); // name
alert( Symbol.keyFor(sym2) ); // id
```

`Symbol.keyFor` utilise en interne le registre de symboles global pour rechercher la clé du symbole. Donc, cela ne fonctionne pas pour les symboles non globaux. Si le symbole n’est pas global, il ne pourra pas le trouver et retournera `undefined`.

Cela dit, tous les symboles ont la propriété `description`.

Par exemple :

```js run
let globalSymbol = Symbol.for("name");
let localSymbol = Symbol("name");

alert( Symbol.keyFor(globalSymbol) ); // name, global symbol
alert( Symbol.keyFor(localSymbol) ); // undefined, not global

alert( localSymbol.description ); // name
```

## System symbols

Il existe de nombreux "systèmes" symboles que JavaScript utilise en interne et que nous pouvons utiliser pour affiner divers aspects de nos objets.

Ils sont listés dans la documentation [Well-known symbols](https://tc39.github.io/ecma262/#sec-well-known-symbols) : 

- `Symbol.hasInstance`
- `Symbol.isConcatSpreadable`
- `Symbol.iterator`
- `Symbol.toPrimitive`
- …etc.

Par exemple, `Symbol.toPrimitive` nous permet de décrire une conversion d’objet en primitive. Nous verrons son utilisation très bientôt.

Nous nous familiariserons également avec d’autres symboles lorsque nous étudierons les caractéristiques du langage correspondantes.

## Résumé

`Symbol` est un type primitif pour les identificateurs uniques.

Les symboles sont créés avec l'appel `Symbol()` ainsi qu'une description facultative.

Les symboles sont toujours de valeurs différentes, même s'ils portent le même nom. Si nous voulons que les symboles portant le même nom soient égaux, nous devons utiliser le registre global : `Symbol.for(key)` renvoie (crée si nécessaire) un symbole global avec `key` comme nom. 
Les multiples appels de `Symbol.for` avec la même `key` renvoient exactement le même symbole.

Les symboles ont deux principaux cas d'utilisation :

1. Propriétés d'objet "masquées".
    Si nous voulons ajouter une propriété à un objet qui "appartient" à un autre script ou à une librairie, nous pouvons créer un symbole et l'utiliser comme clé de propriété. Une propriété symbolique n’apparait pas dans `for..in`, elle ne sera donc pas traitée accidentellement avec d'autres propriétés. De plus, elle ne sera pas accessible directement, car un autre script n’a pas notre symbole. Ainsi, la propriété sera protégée contre une utilisation accidntelle ou un écrasement.

    Ainsi, nous pouvons "dissimuler" quelque chose dans des objets dont nous avons besoin, mais que les autres ne devraient pas voir, en utilisant des propriétés symboliques.

2. De nombreux symboles système utilisés par JavaScript sont accessibles en tant que `Symbol.*`. Nous pouvons les utiliser pour modifier certains comportements internes. Par exemple, plus tard dans le tutoriel, nous utiliserons `Symbol.iterator` pour [iterables](info:iterable), `Symbol.toPrimitive` etc.

Techniquement, les symboles ne sont pas cachés à 100%. Il y a une méthode intégrée [Object.getOwnPropertySymbols(obj)](mdn:js/Object/getOwnPropertySymbols) qui nous permet d’obtenir tous les symboles. Il y a aussi une méthode nommée [Reflect.ownKeys(obj)](mdn:js/Reflect/ownKeys) qui renvoie *toutes* les clés d'un objet, y compris celles symboliques. Donc, ils ne sont pas vraiment cachés. Donc, ils ne sont pas vraiment cachés. Mais la plupart des bibliothèques, fonctions intégrées et structures de syntaxe n'utilisent pas ces méthodes.
