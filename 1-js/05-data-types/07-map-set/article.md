# Map et Set

Jusqu'à présent, nous avons découvert les structures de données complexes suivantes :

- Les objets sont utilisés pour stocker des collections de clés.
- Les tableaux sont utilisés pour stocker des collections ordonnées.

Mais ce n'est pas suffisant pour la vie réelle. C'est pourquoi `Map` et `Set` existent également.

## Map

<<<<<<< HEAD
Une [Map](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Objets_globaux/Map) (dictionnaire de donnée) permet, comme pour un `Object`, de stocker plusieurs éléments sous la forme de clés-valeurs. Sauf que cette fois, les clés peuvent être de n'importe quel type.
=======
[Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) is a collection of keyed data items, just like an `Object`. But the main difference is that `Map` allows keys of any type.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

Voici les méthodes et les propriétés d'une `Map` :

<<<<<<< HEAD
- `new Map()` -- crée la map.
- [`map.set(key, value)`](mdn:js/Map/set) -- stocke la valeur par la clé.
- [`map.get(key)`](mdn:js/Map/get) -- renvoie la valeur par la clé, `undefined` si `key` n'existe pas dans la map.
- [`map.has(key)`](mdn:js/Map/has) -- renvoie `true` si la `key` existe, `false` sinon.
- [`map.delete(key)`](mdn:js/Map/delete) -- supprime la valeur par la clé.
- [`map.clear()`](mdn:js/Map/clear) -- supprime tout de la map.
- [`map.size`](mdn:js/Map/size) -- renvoie le nombre actuel d'éléments.
=======
- [`new Map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/Map) -- creates the map.
- [`map.set(key, value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/set) -- stores the value by the key.
- [`map.get(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/get) -- returns the value by the key, `undefined` if `key` doesn't exist in map.
- [`map.has(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/has) -- returns `true` if the `key` exists, `false` otherwise.
- [`map.delete(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/delete) -- removes the element (the key/value pair) by the key.
- [`map.clear()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/clear) -- removes everything from the map.
- [`map.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/size) -- returns the current element count.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

Par exemple :

```js run
let map = new Map();

map.set('1', 'str1');   // une clé de type chaîne de caractère
map.set(1, 'num1');     // une clé de type numérique
map.set(true, 'bool1'); // une clé de type booléenne

// souvenez-vous, dans un `Object`, les clés sont converties en chaîne de caractères
// alors que `Map` conserve le type d'origine de la clé,
// c'est pourquoi les deux appels suivants retournent des valeurs  :

alert( map.get(1)   ); // 'num1'
alert( map.get('1') ); // 'str1'

alert( map.size ); // 3
```

Au travers de cet exemple nous pouvons voir, qu'à la différence des `Objects`, les clés ne sont pas converties en chaîne de caractère.
Il est donc possible d'utiliser n'importe quel type.

```smart header="`map[key]` n'est pas la bonne façon d'utiliser un `Map`"
Bien que `map[key]` fonctionne également, par exemple nous pouvons définir `map[key] = 2`, cela traite `map` comme un objet JavaScript simple, ce qui implique toutes les limitations correspondantes (uniquement des clés chaîne de caractères/symbol etc.).

Nous devons donc utiliser les méthodes de `map` : `set`, `get` et ainsi de suite.
```

**Map peut également utiliser des objets comme clés.**

Par exemple :

```js run
let john = { name: "John" };

// pour chaque utilisateur, nous stockons le nombre de visites
let visitsCountMap = new Map();

// john est utilisé comme clé dans la map
visitsCountMap.set(john, 123);

alert( visitsCountMap.get(john) ); // 123
```

Utiliser des objets comme clés est l'une des fonctionnalités les plus notables et les plus importantes de `Map`. La même chose ne compte pas pour `Object`. Une chaîne de caractères comme clé dans `Object` est très bien, mais nous ne pouvons pas utiliser un autre `Object` comme clé dans `Object`.

Essayons de faire comme l'exemple précédent directement avec un `Object` :

```js run
let john = { name: "John" };
let ben = { name: "Ben" };

let visitsCountObj = {}; // on créé notre object

visitsCountObj[ben] = 234; // essayez d'utiliser l'objet ben comme clé
visitsCountObj[john] = 123; // essayez d'utiliser l'objet john comme clé, l'objet ben sera remplacé

*!*
// C'est ce qui a été écrit!
alert( visitsCountObj["[object Object]"] ); // 123
*/!*
```

Comme `visitesCountObj` est un objet, il convertit toutes les clés `Object`, telles que `john` et `ben` ci-dessus, en la même chaîne de caractères `"[object Object]"`. Certainement pas ce que nous voulons.

```smart header="Comment `Map` compare les clés"

Pour tester l'égalité entre les clés, `Map` se base sur l'algorithme [SameValueZero](https://tc39.github.io/ecma262/#sec-samevaluezero).
C'est grosso modo la même chose que l'opérateur de stricte égalité `===`, à la différence que `NaN` est considéré comme étant égal à `NaN`.
`NaN` peut donc être utilisé comme clé.

Cet algorithme ne peut pas être modifié.
```

````smart header="Chaînage"

Chaque appel à `map.set` retourne la `map` elle-même, ce qui nous permet d'enchaîner les appels :

```js
map.set('1', 'str1')
  .set(1, 'num1')
  .set(true, 'bool1');
```
````

<<<<<<< HEAD
## Parcourir les éléments d'une `Map`

Il existe 3 façons de parcourir les éléments d'une `map` :
=======
## Iteration over Map
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

- [`map.keys()`](mdn:js/Map/keys) -- renvoie un itérable pour les clés,
- [`map.values()`](mdn:js/Map/values) -- renvoie un itérable pour les valeurs,
- [`map.entries()`](mdn:js/Map/entries) -- renvoie un itérable pour les entrées `[key, value]`, il est utilisé par défaut dans `for..of`.

<<<<<<< HEAD
Par exemple :
=======
- [`map.keys()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/keys) -- returns an iterable for keys,
- [`map.values()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/values) -- returns an iterable for values,
- [`map.entries()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/entries) -- returns an iterable for entries `[key, value]`, it's used by default in `for..of`.

For instance:
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

```js run
let recipeMap = new Map([
  ['cucumber', 500],
  ['tomatoes', 350],
  ['onion',    50]
]);

// on parcourt les clés (les légumes)
for (let vegetable of recipeMap.keys()) {
  alert(vegetable); // cucumber, tomatoes, onion
}

// on parcourt les valeurs (les montants)
for (let amount of recipeMap.values()) {
  alert(amount); // 500, 350, 50
}

// on parcourt les entries (couple [clé, valeur])
for (let entry of recipeMap) { // équivalent à : recipeMap.entries()
  alert(entry); // cucumber,500 (etc.)
}
```

```smart header="L'ordre d'insertion est conservé"
Contraitement aux `Object`, `Map` conserve l'ordre d'insertion des valeurs.
```

Il est aussi possible d'utiliser `forEach` avec `Map` comme on pourrait le faire avec un tableau :

```js
// exécute la fonction pour chaque couple (key, value)
recipeMap.forEach( (value, key, map) => {
  alert(`${key}: ${value}`); // cucumber: 500 etc.
});
```

## Object.entries: Créer une Map à partir d'un objet

Lorsqu'une `Map` est créée, nous pouvons passer un tableau (ou un autre itérable) contenant des paires clé/valeur pour l'initialisation, comme ceci :

```js run
// tableau de paires [clé, valeur]
let map = new Map([
  ['1',  'str1'],
  [1,    'num1'],
  [true, 'bool1']
]);

alert( map.get('1') ); // str1
```

<<<<<<< HEAD
Si nous avons un objet simple et que nous souhaitons en créer une `Map`, nous pouvons utiliser la méthode intégrée [Object.entries(obj)](mdn:js/Object/entries) qui renvoie un tableau de paires clé/valeur pour un objet exactement dans ce format.
=======
If we have a plain object, and we'd like to create a `Map` from it, then we can use built-in method [Object.entries(obj)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries) that returns an array of key/value pairs for an object exactly in that format.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

Nous pouvons donc créer une `Map` à partir d'un objet de la manière suivante :

```js run
let obj = {
  name: "John",
  age: 30
};

*!*
let map = new Map(Object.entries(obj));
*/!*

alert( map.get('name') ); // John
```

Ici, `Object.entries` renvoie le tableau de paires clé/valeur : `[ ["name", "John"], ["age", 30] ]`. C'est ce dont a besoin la `Map`.

## Object.fromEntries: Objet à partir d'une Map

Nous venons de voir comment créer une `Map` à partir d'un objet simple avec `Object.entries(obj)`.

Il existe une méthode `Object.fromEntries` qui fait l'inverse : étant donné un tableau de paires `[clé, valeur]`, elle crée un objet à partir de ces paires :

```js run
let prices = Object.fromEntries([
  ['banana', 1],
  ['orange', 2],
  ['meat', 4]
]);

// maintenant, prices = { banana: 1, orange: 2, meat: 4 }

alert(prices.orange); // 2
```

Nous pouvons utiliser `Object.fromEntries` pour obtenir un objet simple à partir d'une `Map`.

Par exemple, nous stockons les données dans une `Map`, mais nous devons les transmettre à un code tiers qui attend un objet simple.

Voici comment procéder :

```js run
let map = new Map();
map.set('banana', 1);
map.set('orange', 2);
map.set('meat', 4);

*!*
let obj = Object.fromEntries(map.entries()); // créer un objet simple (*)
*/!*

// terminé!
// obj = { banana: 1, orange: 2, meat: 4 }

alert(obj.orange); // 2
```

Un appel à `map.entries()` renvoie un itérable de paires clé/valeur, exactement dans le bon format pour `Object.fromEntries`.

Nous pourrions également raccourcir la ligne (*) :

```js
let obj = Object.fromEntries(map); // .entries() omis
```

C'est la même chose, car `Object.fromEntries` attend un objet itérable en argument. Pas nécessairement un tableau. Et l'itération standard pour une `map` renvoie les mêmes paires clé/valeur que `map.entries()`. Ainsi, nous obtenons un objet simple avec les mêmes clés/valeurs que la `map`.

## Set

<<<<<<< HEAD
`Set` est une liste sans doublons.
=======
A [`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) is a special type collection - "set of values" (without keys), where each value may occur only once.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

Ses principales méthodes sont :

<<<<<<< HEAD
- `new Set(iterable)` -- crée le set et si un objet `iterable` est fourni (généralement un tableau), copie les valeurs de celui-ci dans le set.
- [`set.add(value)`](mdn:js/Set/add) -- ajoute une valeur, renvoie le set lui-même.
- [`set.delete(value)`](mdn:js/Set/delete) -- supprime la valeur, renvoie `true` si `value` existait au moment de l'appel, sinon `false`.
- [`set.has(value)`](mdn:js/Set/has) -- renvoie `true` si la valeur existe dans le set, sinon `false`.
- [`set.clear()`](mdn:js/Set/clear) -- supprime tout du set.
- [`set.size`](mdn:js/Set/size) -- renvoie le nombre actuel d’éléments.
=======
- [`new Set([iterable])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/Set) -- creates the set, and if an `iterable` object is provided (usually an array), copies values from it into the set.
- [`set.add(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/add) -- adds a value, returns the set itself.
- [`set.delete(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/delete) -- removes the value, returns `true` if `value` existed at the moment of the call, otherwise `false`.
- [`set.has(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/has) -- returns `true` if the value exists in the set, otherwise `false`.
- [`set.clear()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/clear) -- removes everything from the set.
- [`set.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/size) -- is the elements count.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

Ce qu'il faut surtout savoir c'est que lorsque l'on appelle plusieurs fois `set.add(value)` avec la même valeur, la méthode ne fait rien.
C'est pourquoi chaque valeur est unique dans un `Set`.

Par exemple, nous souhaitons nous souvenir de tous nos visiteurs. Mais chaque visiteurs doit être unique.

`Set` est exactement ce qu'il nous faut :

```js run
let set = new Set();

let john = { name: "John" };
let pete = { name: "Pete" };
let mary = { name: "Mary" };

// visites, certains utilisateurs viennent plusieurs fois
set.add(john);
set.add(pete);
set.add(mary);
set.add(john);
set.add(mary);

// set conserve une fois chaque visiteurs
alert( set.size ); // 3

for (let user of set) {
  alert(user.name); // John (puis Pete et Mary)
}
```

<<<<<<< HEAD
Nous aurions aussi pu utiliser un tableau (`Array`) en vérifiant avant chaque insertion que l'élément n'existe pas en utilisant [arr.find](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Objets_globaux/Array/find). Cependant les performances auraient été moins bonnes car cette méthode parcours chaque élément du tableau. `Set` est beaucoup plus efficace car il est optimisé en interne pour vérifier l'unicité des valeurs.
=======
The alternative to `Set` could be an array of users, and the code to check for duplicates on every insertion using [arr.find](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find). But the performance would be much worse, because this method walks through the whole array checking every element. `Set` is much better optimized internally for uniqueness checks.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

## Parcourir un Set

Nous pouvons parcourir les éléments d'un Set avec `for..of` ou en utilisant `forEach` :

```js run
let set = new Set(["oranges", "apples", "bananas"]);

for (let value of set) alert(value);

// même chose en utilisant forEach:
set.forEach((value, valueAgain, set) => {
  alert(value);
});
```

A noter que la fonction de callback utilisée par `forEach` prend 3 arguments en paramètres : une `value`, puis *la même valeur* `valueAgain`, et enfin le set lui-même.


C'est pour la compatibilité avec `Map` où le callback `forEach` passé possède trois arguments. Ça a l'air un peu étrange, c'est sûr. Mais cela peut aider à remplacer facilement `Map` par `Set` dans certains cas, et vice versa.

Les méthodes pour parcourir les éléments d'une `Map` peuvent être utilisées :

<<<<<<< HEAD
- [`set.keys()`](mdn:js/Set/keys) -- renvoie un objet itérable pour les valeurs,
- [`set.values()`](mdn:js/Set/values) -- identique à `set.keys()`, pour compatibilité avec `Map`,
- [`set.entries()`](mdn:js/Set/entries) -- renvoie un objet itérable pour les entrées `[value, value]`, existe pour la compatibilité avec `Map`.
=======
- [`set.keys()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/keys) -- returns an iterable object for values,
- [`set.values()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/values) -- same as `set.keys()`, for compatibility with `Map`,
- [`set.entries()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/entries) -- returns an iterable object for entries `[value, value]`, exists for compatibility with `Map`.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

## Résumé

<<<<<<< HEAD
`Map` -- est une collection de clé valeurs.
=======
[`Map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) -- is a collection of keyed values.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

Méthodes et propriétés :

<<<<<<< HEAD
- `new Map([iterable])` -- crée la map, avec un `iterable` facultatif (par exemple un tableau) de paires `[key,value]` pour l'initialisation.
- [`map.set(key, value)`](mdn:js/Map/set) -- stocke la valeur par la clé, renvoie la map elle-même.
- [`map.get(key)`](mdn:js/Map/get) -- renvoie la valeur par la clé, `undefined` si `key` n'existe pas dans la map.
- [`map.has(key)`](mdn:js/Map/has) -- renvoie `true` si la `key` existe, `false` sinon.
- [`map.delete(key)`](mdn:js/Map/delete) -- supprime la valeur par la clé, retourne `true` si `key` existait au moment de l'appel, sinon `false`.
- [`map.clear()`](mdn:js/Map/clear) -- supprime tout de la map.
- [`map.size`](mdn:js/Map/size) -- renvoie le nombre actuel d'éléments.
=======
- [`new Map([iterable])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/Map) -- creates the map, with optional `iterable` (e.g. array) of `[key,value]` pairs for initialization.
- [`map.set(key, value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/set) -- stores the value by the key, returns the map itself.
- [`map.get(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/get) -- returns the value by the key, `undefined` if `key` doesn't exist in map.
- [`map.has(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/has) -- returns `true` if the `key` exists, `false` otherwise.
- [`map.delete(key)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/delete) -- removes the element by the key, returns `true` if `key` existed at the moment of the call, otherwise `false`.
- [`map.clear()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/clear) -- removes everything from the map.
- [`map.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map/size) -- returns the current element count.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

La différence entre `Map` avec un objet traditionel :

- N'importe quel type peut être utilisé comme clé.
- Accès à des méthodes tels que `size`.

<<<<<<< HEAD
`Set` -- est une collection de valeurs uniques.
=======
[`Set`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) -- is a collection of unique values.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

Méthodes et propriétés :

<<<<<<< HEAD
- `new Set([iterable])` -- crée le set avec un `iterable` facultatif (par exemple un tableau) de valeurs pour l'initialisation.
- [`set.add(value)`](mdn:js/Set/add) -- ajoute une valeur (ne fait rien si `value` existe), renvoie le set lui-même.
- [`set.delete(value)`](mdn:js/Set/delete) -- supprime la valeur, renvoie `true` si `value` existait au moment de l'appel, sinon `false`.
- [`set.has(value)`](mdn:js/Set/has) -- renvoie `true` si la valeur existe dans le set sinon `false`.
- [`set.clear()`](mdn:js/Set/clear) -- supprime tout du set.
- [`set.size`](mdn:js/Set/size) -- renvoie le nombre actuel d’éléments.
=======
- [`new Set([iterable])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/Set) -- creates the set, with optional `iterable` (e.g. array) of values for initialization.
- [`set.add(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/add) -- adds a value (does nothing if `value` exists), returns the set itself.
- [`set.delete(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/delete) -- removes the value, returns `true` if `value` existed at the moment of the call, otherwise `false`.
- [`set.has(value)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/has) -- returns `true` if the value exists in the set, otherwise `false`.
- [`set.clear()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/clear) -- removes everything from the set.
- [`set.size`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set/size) -- is the elements count.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

On ne peut pas dire que les éléments dans une `Map` ou un `Set` sont désordonnés car ils sont toujours parcourut par ordre d'insertion.
Il est cependant impossible de réorganiser les éléments ou bien de les retrouver par leur index.
