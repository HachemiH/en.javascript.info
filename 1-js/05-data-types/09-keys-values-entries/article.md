# Object.keys, values, entries

Éloignons-nous des structures de données individuelles et parlons des itérations les concernant.

Dans le chapitre précédent, nous avons vu les méthodes `map.keys()`, `map.values()`, `map.entries()`.

Ces méthodes sont génériques, il existe une convention de les utiliser pour les structures de données. Si nous devions créer notre propre structure de données, nous devrions aussi les implémenter.

Ils sont pris en charge par:

- `Map`
- `Set`
- `Array` 

Les objets simples prennent également en charge des méthodes similaires, mais la syntaxe est un peu différente.

## Object.keys, values, entries

Pour les objets simples, les méthodes suivantes sont disponibles:

- [Object.keys(obj)](mdn:js/Object/keys) -- retourne un tableau de clés.
- [Object.values(obj)](mdn:js/Object/values) -- retourne un tableau de valeurs.
- [Object.entries(obj)](mdn:js/Object/entries) -- retourne un tableau de paires `[clé, valeur]`.

Veuillez noter les différences (par rapport à map par exemple):

|             | Map              | Object       |
|-------------|------------------|--------------|
| Syntaxe d'appel | `map.keys()`  | `Object.keys(obj)`, mais pas `obj.keys()` |
| Retours     | iterable    | "vrai" Array                     |

La première différence est que nous devons appeler `Object.keys(obj)` et non `obj.keys()`.

Pourquoi cela ? La principale raison est la flexibilité. N'oubliez pas que les objets sont une base de toutes les structures complexes en JavaScript. Ainsi, nous pouvons avoir un objet à nous comme `data` qui implémente sa propre méthode `data.values()`. Et nous pouvons toujours appeler `Object.values(data)`.

La seconde différence réside dans le fait que les méthodes `Object.*` retourne de "réels" objets de tableau, et pas seulement des objets itératifs. C'est principalement pour des raisons historiques.

Par exemple:

```js
let user = {
  name: "John",
  age: 30
};
```

- `Object.keys(user) = [name, age]`
- `Object.values(user) = ["John", 30]`
- `Object.entries(user) = [ ["name","John"], ["age",30] ]`

Voici un exemple d'utilisation d'`Object.values` pour boucler les valeurs de propriété:

```js run
let user = {
  name: "John",
  age: 30
};

// boucle sur les valeurs
for (let value of Object.values(user)) {
  alert(value); // John, ensuite 30
}
```

```warn header="Object.keys/values/entries ignorer les propriétés symboliques"

Tout comme une boucle `for..in`, ces méthodes ignorent les propriétés qui utilisent `Symbol(...)` comme clés.

D'habitude c'est pratique. Mais si nous voulons aussi des clés symboliques, il existe une méthode distincte [Object.getOwnPropertySymbols](mdn:js/Object/getOwnPropertySymbols) qui retourne un tableau composé uniquement de clés symboliques. De plus, la méthde [Reflect.ownKeys(obj)](mdn:js/Reflect/ownKeys) renvoie *toutes* les clés.
```

## Transformer des objets

Les objets manquent de nombreuses méthodes existantes pour les tableaux, par exemple `map`, `filter` et autres.

Si nous souhaitons leur appliquer ces méthodes, nous pouvons utiliser `Object.entries` suivis par `Object.fromEntries` :

<<<<<<< HEAD
1. Utilisons `Object.entries(obj)` pour obtenir un tableau de paires clé / valeur de `obj`.
2. Utilisons des méthodes de tableau sur ce tableau, par exemple `map`.
3. Utilisons `Object.fromEntries(array)` sur le tableau résultant pour le reconvertir en objet.
=======
1. Use `Object.entries(obj)` to get an array of key/value pairs from `obj`.
2. Use array methods on that array, e.g. `map`, to transform these key/value pairs.
3. Use `Object.fromEntries(array)` on the resulting array to turn it back into an object.
>>>>>>> ef8d576821ff28c69bfb7410dc79fd216b0a315b

Par exemple, nous avons un objet avec des prix et aimerions les doubler :

```js run
let prices = {
  banana: 1,
  orange: 2,
  meat: 4,
};

*!*
let doublePrices = Object.fromEntries(
<<<<<<< HEAD
  // convertir en tableau, mapper, puis fromEntries restitue l'objet
  Object.entries(prices).map(([key, value]) => [key, value * 2])
=======
  // convert prices to array, map each key/value pair into another pair
  // and then fromEntries gives back the object
  Object.entries(prices).map(entry => [entry[0], entry[1] * 2])
>>>>>>> ef8d576821ff28c69bfb7410dc79fd216b0a315b
);
*/!*

alert(doublePrices.meat); // 8
```

<<<<<<< HEAD
Cela peut sembler difficile au premier abord, mais il devient facile à comprendre après l’avoir utilisé une ou deux fois. Nous pouvons faire de puissantes chaînes de transformations de cette façon.
=======
It may look difficult at first sight, but becomes easy to understand after you use it once or twice. We can make powerful chains of transforms this way.
>>>>>>> ef8d576821ff28c69bfb7410dc79fd216b0a315b
