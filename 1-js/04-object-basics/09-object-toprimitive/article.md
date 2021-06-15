
# Conversion d'objet en primitive

Que se passe-t-il lorsque des objets sont ajoutés `obj1 + obj2`, soustraits `obj1 - obj2` ou imprimés à l'aide de `alert (obj)` ?

<<<<<<< HEAD
Dans ce cas, les objets sont automatiquement convertis en primitives, puis l'opération est effectuée.
=======
JavaScript doesn't exactly allow to customize how operators work on objects. Unlike some other programming languages, such as Ruby or C++, we can't implement a special object method to handle an addition (or other operators).

In case of such operations, objects are auto-converted to primitives, and then the operation is carried out over these primitives and results in a primitive value.

That's an important limitation, as the result of `obj1 + obj2` can't be another object!

E.g. we can't make objects representing vectors or matrices (or archievements or whatever), add them and expect a "summed" object as the result. Such architectural feats are automatically "off the board".

So, because we can't do much here, there's no maths with objects in real projects. When it happens, it's usually because of a coding mistake.

In this chapter we'll cover how an object converts to primitive and how to customize it.

We have two purposes:

1. It will allow us to understand what's going on in case of coding mistakes, when such an operation happened accidentally.
2. There are exceptions, where such operations are possible and look good. E.g. subtracting or comparing dates (`Date` objects). We'll come across them later.

## Conversion rules
>>>>>>> fb4fc33a2234445808100ddc9f5e4dcec8b3d24c

Dans le chapitre <info:type-conversions> nous avons vu les règles pour les conversions numériques, chaînes et booléennes de primitives. Mais nous avions mis de côté les objets. Maintenant que nous connaissons les méthodes et les symboles, il devient possible de l'aborder.

Pour les objets, il n’y a pas de conversion to-boolean, car tous les objets sont `true` dans un contexte booléen. Il n'y a donc que des conversions de chaînes de caractères et de chiffres.

1. Tous les objets sont `true` dans un contexte booléen. Il n'y a que des conversions numériques et de chaînes de caractères.
2. La conversion numérique se produit lorsque nous soustrayons des objets ou appliquons des fonctions mathématiques. Par exemple, les objets `Date` (à traiter dans le chapitre <info:date>) peut être soustrait et le résultat de `date1 - date2` est la différence de temps entre deux dates.
3. En ce qui concerne la conversion de chaîne de caractères - cela se produit généralement lorsque nous affichons un objet tel que `alert (obj)` et dans des contextes similaires.


<<<<<<< HEAD
## ToPrimitive

Nous pouvons affiner la conversion de chaînes de caractères et de chiffres en utilisant des méthodes d’objet spéciales.

Il existe trois variantes de conversion de type, appelées "hints", décrites dans la [specification](https://tc39.github.io/ecma262/#sec-toprimitive) :
=======
We can fine-tune string and numeric conversion, using special object methods.

There are three variants of type conversion, that happen in various situations.

They're called "hints", as described in the [specification](https://tc39.github.io/ecma262/#sec-toprimitive):
>>>>>>> fb4fc33a2234445808100ddc9f5e4dcec8b3d24c


**`"string"`**

Pour une conversion d'un objet vers une chaîne de caractères, lorsque nous effectuons une opération sur un objet qui attend une chaîne, comme `alert` :

```js
// output
alert(obj);

// utiliser un objet comme clé de propriété
anotherObj[obj] = 123;
```


**`"number"`**

Pour une conversion d'objet en nombre, comme lorsque nous faisons des calculs :

```js
// conversion explicite
let num = Number(obj);

// maths (except binary plus)
let n = +obj; // unary plus
let delta = date1 - date2;

// comparaison supérieur/inférieur
let greater = user1 > user2;
```

`"default"`
: Se produit dans de rares cas où l'opérateur n'est "pas sûr" du type auquel il doit s'attendre.

    Par exemple, le binaire plus `+` peut fonctionner à la fois avec des chaînes de caractères (les concaténer) et des nombres (les ajouter), donc les chaînes de caractères et les chiffres feraient l'affaire. Donc, si le plus binaire obtient un objet sous forme d'argument, il utilise l'indicateur `"default"` pour le convertir.

    En outre, si un objet est comparé à l'aide de `==` avec une chaîne de caractères, un nombre ou un symbole, il est également difficile de savoir quelle conversion doit être effectuée, par conséquent l'indicateur `"default"` est utilisé.

    ```js
    // binary plus uses the "default" hint
    let total = obj1 + obj2;

    // obj == number uses the "default" hint
    if (user == 1) { ... };
    ```

    Les opérateurs de comparaison supérieurs et inférieurs, tels que `<` `>`, peuvent également fonctionner avec des chaînes de caractères et des nombres. Néanmoins, ils utilisent l'indicateur `"number"`, pas `default`. C'est pour des raisons historiques,

    En pratique cependant, nous n'avons pas besoin de nous souvenir de ces détails particuliers, car tous les objets intégrés, à l'exception d'un cas (l'objet `Date`, nous l'apprendrons plus tard), implémentent la conversion `'default'` de la même manière que `"number"`. Et nous pouvons faire la même chose.

```smart header="Pas d'indice `\"boolean\"`"
Veuillez noter qu'il n'y a que trois indices. C'est aussi simple que cela.

Il n'y a pas d'indice "boolean" (tous les objets sont `true` dans un contexte booléen) ou autre chose. Et si nous traitons de la même manière `'default'` et `'number'`, comme le font la plupart des programmes intégrés, il n'y a au final que deux conversions.
```

**Pour effectuer la conversion, JavaScript essaie de trouver et d'appeler trois méthodes d'objet :**

1. Appeler `obj[Symbol.toPrimitive](hint)` - la méthode avec la clé symbolique `Symbol.toPrimitive` (symbole système), si une telle méthode existe,
2. Sinon, si l'indice est `"string"`
    - essaie `obj.toString()` et `obj.valueOf()`, tout ce qui existe.
3. Sinon, si l'indice est `"number"` ou `"default"`
    - essaie `obj.valueOf()` et `obj.toString()`, tout ce qui existe.

## Symbol.toPrimitive

Commençons par la première méthode. Il existe un symbole intégré appelé `Symbol.toPrimitive` qui devrait être utilisé pour nommer la méthode de conversion, comme ceci :

```js
obj[Symbol.toPrimitive] = function(hint) {
<<<<<<< HEAD
  // doit renvoyer une valeur primitive
  // hint = un parmi "string", "number", "default"
};
```

Par exemple, ici l'objet `user` l'implémente :
=======
  // here goes the code to convert this object to a primitive
  // it must return a primitive value
  // hint = one of "string", "number", "default"
};
```

If the method `Symbol.toPrimitive` exists, it's used for all hints, and no more methods are needed.

For instance, here `user` object implements it:
>>>>>>> fb4fc33a2234445808100ddc9f5e4dcec8b3d24c

```js run
let user = {
  name: "John",
  money: 1000,

  [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  }
};

// conversions demo:
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500
```

Comme on peut le voir d'après le code, `user` devient une chaîne de caractères auto-descriptive ou un montant d'argent en fonction de la conversion. La méthode unique `user[Symbol.toPrimitive]` gère tous les cas de conversion.


## toString/valueOf

<<<<<<< HEAD
Méthodes `toString` et `valueOf` proviennent des temps anciens. Ce ne sont pas des symboles (les symboles n'existaient pas il n'y a pas si longtemps), mais plutôt des méthodes "régulières" avec des noms de chaînes de caractères. Ils fournissent une méthode alternative "à l'ancienne" pour implémenter la conversion.

S'il n'y a pas de `Symbol.toPrimitive`, alors JavaScript essaye de les trouver et essaie dans l'ordre :

- `toString -> valueOf` pour le hint "string".
- `valueOf -> toString` sinon.
=======
If there's no `Symbol.toPrimitive` then JavaScript tries to find methods `toString` and `valueOf`:

- For the "string" hint: `toString`, and if it doesn't exist, then `valueOf` (so `toString` has the priority for stirng conversions).
- For other hints: `valueOf`, and if it doesn't exist, then `toString` (so `valueOf` has the priority for maths).

Methods `toString` and `valueOf` come from ancient times. They are not symbols (symbols did not exist that long ago), but rather "regular" string-named methods. They provide an alternative "old-style" way to implement the conversion.
>>>>>>> fb4fc33a2234445808100ddc9f5e4dcec8b3d24c

Ces méthodes doivent renvoyer une valeur primitive. Si `toString` ou `valueOf` renvoie un objet, il est ignoré (comme s'il n'y avait pas de méthode).

Par défaut, un objet brut a les méthodes `toString` et `valueOf` suivantes :

- La méthode `toString` renvoie une chaîne de caractères `"[object Object]"`.
- La méthode `valueOf` renvoie l'objet en question.

Voici la démo :

```js run
let user = {name: "John"};

alert(user); // [object Object]
alert(user.valueOf() === user); // true
```

Donc, si nous essayons d'utiliser un objet en tant que chaîne de caractères, comme dans un `alert` ou autre chose, nous voyons par défaut `[object Object]`.

<<<<<<< HEAD
Et la valeur par défaut `valueOf` n'est mentionnée ici que par souci d'exhaustivité, afin d'éviter toute confusion. Comme vous pouvez le constater, l'objet est renvoyé et est donc ignoré. Ne me demandez pas pourquoi, c'est pour des raisons historiques. Nous pouvons donc supposer que cela n'existe pas.

Implémentons ces méthodes.
=======
The default `valueOf` is mentioned here only for the sake of completeness, to avoid any confusion. As you can see, it returns the object itself, and so is ignored. Don't ask me why, that's for historical reasons. So we can assume it doesn't exist.

Let's implement these methods to customize the conversion.
>>>>>>> fb4fc33a2234445808100ddc9f5e4dcec8b3d24c

Par exemple, ici, `user` fait la même chose que ci-dessus en combinant `toString` et `valueOf` au lieu de `Symbol.toPrimitive` :

```js run
let user = {
  name: "John",
  money: 1000,

  // for hint="string"
  toString() {
    return `{name: "${this.name}"}`;
  },

  // pour hint="number" ou "default"
  valueOf() {
    return this.money;
  }

};

alert(user); // toString -> {name: "John"}
alert(+user); // valueOf -> 1000
alert(user + 500); // valueOf -> 1500
```

Comme on peut le constater, le comportement est identique à celui de l'exemple précédent avec `Symbol.toPrimitive`.

Nous voulons souvent un seul endroit "fourre-tout" pour gérer toutes les conversions primitives. Dans ce cas, nous pouvons implémenter `toString` uniquement, comme ceci :

```js run
let user = {
  name: "John",

  toString() {
    return this.name;
  }
};

alert(user); // toString -> John
alert(user + 500); // toString -> John500
```

En l'absence de `Symbol.toPrimitive` et de `valueOf`, `toString` gérera toutes les conversions primitives.

<<<<<<< HEAD
## Retourner des types
=======
### A conversion can return any primitive type
>>>>>>> fb4fc33a2234445808100ddc9f5e4dcec8b3d24c

La chose importante à savoir sur toutes les méthodes de conversion de primitives est qu'elles ne renvoient pas nécessairement la primitive "hinted".

Il n'y a pas de control pour vérifier si `ToString()` renvoie exactement une chaîne de caractères ou si la méthode `Symbol.toPrimitive` renvoie un nombre pour un indice `"number"`.

**La seule chose obligatoire : ces méthodes doivent renvoyer une primitive, pas un objet.**

```smart header="Notes historiques"
Pour des raisons historiques, si `toString` ou `valueOf` renvoie un objet, il n’y a pas d’erreur, mais cette valeur est ignorée (comme si la méthode n’existait pas). C’est parce que jadis, il n’existait pas de bon concept "d’erreur" en JavaScript.

En revanche, `Symbol.toPrimitive` doit renvoyer une primitive, sinon une erreur se produira.
```

## Autres conversions

Comme nous le savons déjà, de nombreux opérateurs et fonctions effectuent des conversions de types, par exemple la multiplication `*` convertit les opérandes en nombres.

Si nous passons un objet en argument, il y a deux étapes :
1. L'objet est converti en primitive (en utilisant les règles décrites ci-dessus).
2. Si la primitive résultante n'est pas du bon type, elle est convertie.

Par exemple :

```js run
let obj = {
  // toString gère toutes les conversions en l'absence d'autres méthodes
  toString() {
    return "2";
  }
};

alert(obj * 2); // 4, objet converti en primitive "2", puis la multiplication le transforme en un nombre
```

1. La multiplication `obj * 2` convertit d'abord l'objet en primitive (cela devient une chaîne de caractère `"2"`).
2. Ensuite `"2" * 2` devient `2 * 2` (la chaîne de caractères est convertie en nombre).

Le binaire plus va concaténer des chaînes de caractères dans la même situation, car il accepte volontiers une chaîne de caractères :

```js run
let obj = {
  toString() {
    return "2";
  }
};

alert(obj + 2); // 22 ("2" + 2), la conversion en primitive a renvoyé une chaîne de caractères => concaténation
```

## Résumé

La conversion objet à primitive est appelée automatiquement par de nombreuses fonctions intégrées et opérateurs qui attendent une primitive en tant que valeur.

Il en existe 3 types (hints) :
- [ ] `"string"` (pour `alert` et d'autres opérations qui nécessitent une chaîne de caractères)
- `"number"` (pour des maths)
- `"default"` (peu d'opérateurs)

La spécification décrit explicitement quel opérateur utilise quel indice (hint). Très peu d’opérateurs "ne savent pas à quoi s’attendre" et utilisent l'indice `"default"`. Habituellement, pour les objets intégrés, l'indice `"default"` est traité de la même façon que `"number"`, de sorte qu'en pratique, les deux derniers sont souvent fusionnés.

L'algorithme de conversion est :

1. Appeler `obj[Symbol.toPrimitive](hint)` si la méthode existe,
2. Sinon, si l'indice est `"string"`
    - essaie `obj.toString()` et `obj.valueOf()`, tout ce qui existe.
3. Sinon, si l'indice est `"number"` ou `"default"`
    - essaie `obj.valueOf()` et `obj.toString()`, tout ce qui existe.

<<<<<<< HEAD
En pratique, il suffit souvent d’implémenter uniquement `obj.toString()` en tant que méthode "fourre-tout" pour toutes les conversions qui renvoient une représentation "lisible par l’homme" d’un objet, à des fins de journalisation ou de débogage.
=======
In practice, it's often enough to implement only `obj.toString()` as a "catch-all" method for string conversions that should return a "human-readable" representation of an object, for logging or debugging purposes.  

As for math operations, JavaScript doesn't provide a way to "override" them using methods, so real life projects rarely use them on objects.
>>>>>>> fb4fc33a2234445808100ddc9f5e4dcec8b3d24c
