
# Objets

Comme nous le savons du chapitre <info:types>, il existe sept types de données dans le langage JavaScript. Six d'entre elles sont appelées "primitives", car leurs valeurs ne contiennent qu'une seule chose (que ce soit une chaîne, un nombre ou autre).

En revanche, les objets sont utilisés pour stocker des collections de données variées et d’entités plus complexes. En JavaScript, les objets pénètrent dans presque tous les aspects du langage. Nous devons donc d'abord les comprendre avant d'aller plus loin.

Un objet peut être créé avec des accolades `{…}`, avec une liste optionnelle de *propriétés*. Une propriété est une paire "clé: valeur", dans laquelle la clé (`key`) est une chaîne de caractères (également appelée "nom de la propriété"), et la valeur (`value`) peut être n'importe quoi.

Nous pouvons imaginer un objet comme une armoire avec des fichiers signés. Chaque donnée est stockée dans son fichier par la clé. Il est facile de trouver un fichier par son nom ou d’ajouter/supprimer un fichier.

![](object.svg)

Un objet vide ("armoire vide") peut être créé en utilisant l'une des deux syntaxes suivantes :

```js
let user = new Object(); // syntaxe "constructeur d'objet"
let user = {};  // syntaxe "littéral objet"
```

![](object-user-empty.svg)

Habituellement, les accolades `{...}` sont utilisées. Cette déclaration s'appelle un littéral objet (*object literal*).

## Littéraux et propriétés

Nous pouvons immédiatement inclure certaines propriétés dans `{...}` sous forme de paires "clé: valeur" :

```js
let user = {     // un objet
  name: "John",  // par clé "nom" valeur de stockage "John"
  age: 30        // par clé "age" valeur de stockage 30
};
```

Une propriété a une clé (également appelée "nom" ou "identifiant") avant les deux points `":"` et une valeur à sa droite.

Dans l'objet `user`, il y a deux propriétés :

1. La première propriété porte le nom `"name"` et la valeur `"John"`.
2. La seconde a le nom `"age"` et la valeur `30`.

L'objet `user` résultant peut être imaginé comme une armoire avec deux fichiers signés intitulés "nom" et "âge".

![user object](object-user.svg)

Nous pouvons ajouter, supprimer et lire des fichiers à tout moment.

Les valeurs de propriété sont accessibles à l'aide de la notation par points :

```js
// récupère les valeurs de propriété de l'objet :
alert( user.name ); // John
alert( user.age ); // 30
```

La valeur peut être de tout type. Ajoutons un booléen :

```js
user.isAdmin = true;
```

![user object 2](object-user-isadmin.svg)

Pour supprimer une propriété, nous pouvons utiliser l'opérateur `delete` :

```js
delete user.age;
```

![user object 3](object-user-delete.svg)

Nous pouvons également utiliser des noms de propriété multi-mots, mais ils doivent ensuite être entourés de quotes :

```js
let user = {
  name: "John",
  age: 30,
  "likes birds": true  // le nom de la propriété multi-mots doit être entourée de quotes
};
```

![](object-user-props.svg)


La dernière propriété de la liste peut se terminer par une virgule :
```js
let user = {
  name: "John",
  age: 30*!*,*/!*
}
```
Cela s'appelle une virgule  "trailing" ou "hanging". Elle facilite l'ajout/suppression/déplacement des propriétés, car toutes les lignes se ressemblent.

## Crochets

Pour les propriétés multi-mots, l’accès par points ne fonctionne pas :

```js run
// cela donnerait une erreur de syntaxe
user.likes birds = true
```

C’est parce que le point exige que la clé soit un identificateur de variable valide. C'est-à-dire : pas d'espaces et autres limitations.

Il existe une autre “notation entre crochets” qui fonctionne avec n’importe quelle chaîne :

```js run
let user = {};

// set
user["likes birds"] = true;

// get
alert(user["likes birds"]); // true

// delete
delete user["likes birds"];
```

Maintenant tout va bien. Veuillez noter que la chaîne de caractères entre crochets est correctement entourée de quotes (tout type de guillemets fera l'affaire).

Les crochets fournissent également un moyen d'obtenir le nom de la propriété comme résultat de toute expression (par opposition à une chaîne de caractères littérale), semblable à une variable, comme ceci :

```js
let key = "likes birds";

// pareil que user["likes birds"] = true;
user[key] = true;
```

Ici, la variable `key` peut être calculée au moment de l'exécution ou dépendre de la saisie de l'utilisateur. Et ensuite, nous l'utilisons pour accéder à la propriété. Cela nous donne beaucoup de flexibilité. 

Par exemple :

```js run
let user = {
  name: "John",
  age: 30
};

let key = prompt("What do you want to know about the user?", "name");

// accès par variable
alert( user[key] ); // John (si entré "name")
```

The dot notation cannot be used in a similar way:

```js run
let user = {
  name: "John",
  age: 30
};

let key = "name";
user.key // undefined
```

### Propriétés calculées

Nous pouvons utiliser des crochets dans un objet littéral. Cela s'appelle des propriétés calculées (*computed propertie*).

Par exemple :

```js run
let fruit = prompt("Which fruit to buy?", "apple");

let sac = {
*!*
  [fruit]: 5, // le nom de la propriété est tiré de la variable fruit
*/!*
};

alert( bag.apple ); // 5 si fruit="apple"
```

La signification d'une propriété calculée est simple: `[fruit]` signifie que le nom de la propriété doit être extrait de `fruit`.

Ainsi, si un visiteur entre `"apple"`, `bag` deviendra `{apple: 5}`.

Essentiellement, cela fonctionne de la même façon que :
```js run
let fruit = prompt("Which fruit to buy?", "apple");
let bag = {};

// prendre le nom de la propriété de la variable fruit
bag[fruit] = 5;
```

… Mais a une meilleure apparence.

Nous pouvons utiliser des expressions plus complexes entre crochets :

```js
let fruit = 'apple';
let bag = {
  [fruit + 'Computers']: 5 // bag.appleComputers = 5
};
```

Les crochets sont beaucoup plus puissants que la notation par points. Ils autorisent tous les noms de propriété et variables. Mais ils sont aussi plus lourds à écrire.

Ainsi, la plupart du temps, lorsque les noms de propriété sont connus et simples, le point est utilisé. Et si nous avons besoin de quelque chose de plus complexe, nous passons aux crochets.



````smart header="Les mots réservés sont autorisés comme noms de propriétés"
Une variable ne peut pas avoir un nom égal à l'un des mots réservés à la langue tels que "for", "let", "return", etc.

Mais pour une propriété d’objet, il n’ya pas de telle restriction. N'importe quel nom est autorisé :

```js run
let obj = {
  for: 1,
  let: 2,
  return: 3
}

alert( obj.for + obj.let + obj.return );  // 6
```

En principe, tout nom est autorisé, mais il en existe un spécial: `"__proto__"` qui bénéficie d’un traitement spécial pour des raisons historiques. Par exemple, nous ne pouvons pas définir une valeur non-objet :

```js run
let obj = {};
obj.__proto__ = 5;
alert(obj.__proto__); // [object Object], n'a pas fonctionné comme prévu
```

Comme on le voit d'après le code, l'affectation à une primitive `5` est ignorée.

Cela peut devenir une source de bugs et même de vulnérabilités si nous avons l’intention de stocker des paires clé-valeur arbitraires dans un objet et d’autoriser un visiteur à spécifier les clés.

Dans ce cas, le visiteur peut choisir `__proto__` comme clé et la logique d’attribution sera ruinée (comme affiché ci-dessus).

Il existe un moyen de faire en sorte que les objets traitent `__proto__` comme une propriété régulière, ce que nous verrons plus tard, mais nous devons d’abord en savoir plus sur les objets.

Il existe également une autre structure de données [Map](info:map-set-weakmap-weakset), que nous apprendrons dans le chapitre <info:map-set-weakmap-weakset>, qui supporte des clés arbitraires.
````


## Valeur de propriété abrégée (Property value shorthand)

Dans le code, nous utilisons souvent des variables existantes en tant que valeurs pour les noms de propriétés.

Par exemple :

```js run
function makeUser(name, age) {
  return {
    name: name,
    age: age
    // ...autres propriétés
  };
}

let user = makeUser("John", 30);
alert(user.name); // John
```

Dans l'exemple ci-dessus, les propriétés portent les mêmes noms que les variables. Le cas d’utilisation de la création d’une propriété à partir d’une variable est si courant qu’il existe une valeur spéciale de propriété abrégée (*property value shorthand*) pour la rendre plus courte.

Au lieu de `name:name`, nous pouvons simplement écrire `name`, comme ceci :

```js
function makeUser(name, age) {
*!*
  return {
    name, // pareil que name: name
    age   // pareil que age: age
    // ...
  };
*/!*
}
```

Nous pouvons utiliser à la fois des propriétés normales et des raccourcis dans le même objet :

```js
let user = {
  name,  // pareil que name:name
  age: 30
};
```

## Vérification d'existence

Une caractéristique notable des objets est qu’il est possible d’accéder à n’importe quelle propriété. Il n’y aura pas d’erreur si la propriété n’existe pas ! L'accès à une propriété non existante renvoie simplement `undefined`. Le langage fournit un moyen très courant de vérifier si la propriété existe -- pour l'obtenir et la comparer avec `undefined` :

```js run
let user = {};

alert( user.noSuchProperty === undefined ); // true signifie "pas une telle propriété"
```

Il existe également un opérateur spécial `"in"` pour vérifier l'existence d'une propriété.

La syntax est :
```js
"key" in object
```

Par exemple :

```js run
let user = { name: "John", age: 30 };

alert( "age" in user ); // true, user.age existe
alert( "blabla" in user ); // false, user.blabla n'existe pas
```

Veuillez noter que sur le côté gauche de `in`, il doit y avoir un *nom de propriété*. C’est généralement une chaîne de caractères entre guillemets.

Si nous omettons les guillemets, cela signifierait une variable contenant le nom à tester. Par exemple :

```js run
let user = { age: 30 };

let key = "age";
alert( *!*key*/!* in user ); // true, prend le nom de la clé et vérifie cette propriété
```

<<<<<<< HEAD
````smart header="Utilisation de \"in\" pour les propriétés qui stockent `undefined`"
Habituellement, la comparaison stricte `"=== undefined"` fonctionne correctement. Mais il y a un cas particulier où elle échoue, mais "in" fonctionne correctement.
=======
````smart header="Using \"in\" for properties that store `undefined`"
Usually, the strict comparison `"=== undefined"` check the property existance just fine. But there's a special case when it fails, but `"in"` works correctly.
>>>>>>> 34e9cdca3642882bd36c6733433a503a40c6da74

C’est lorsque une propriété d’objet existe, mais qu'elle stocke undefined :

```js run
let obj = {
  test: undefined
};

alert( obj.test ); // c'est indéfini, donc - pas une telle propriété ?

alert( "test" in obj ); // true, la propriété existe !
```


Dans le code ci-dessus, la propriété `obj.test` existe techniquement. Donc, l'opérateur `in` fonctionne bien.

Des situations comme celle-ci se produisent très rarement, parce que `undefined` n'est généralement pas attribué. Nous utilisons principalement `null` pour les valeurs "inconnues" ou "vides". Ainsi, l'opérateur `in` est un invité exotique dans le code.
````

<<<<<<< HEAD

## La boucle "for … in"
=======
## The "for..in" loop
>>>>>>> 34e9cdca3642882bd36c6733433a503a40c6da74

Pour parcourir toutes les clés d'un objet, il existe une forme spéciale de boucle : `for..in`. C'est une chose complètement différente de la construction `for(;;)` que nous avons étudiée auparavant.

La syntaxe :

```js
for(key in object) {
  // exécute le corps pour chaque clé parmi les propriétés de l'objet
}
```

Par exemple, affichons toutes les propriétés de `user` :

```js run
let user = {
  name: "John",
  age: 30,
  isAdmin: true
};

for(let key in user) {
  // keys
  alert( key );  // name, age, isAdmin
  // valeurs pour les clés
  alert( user[key] ); // John, 30, true
}
```

Notez que toutes les constructions "for" nous permettent de déclarer la variable en boucle à l'intérieur de la boucle, comme `let key` ici.

En outre, nous pourrions utiliser un autre nom de variable ici au lieu de `key`. Par exemple, `"for(let prop in obj)"` est également largement utilisé.


### Ordonné comme un objet

Les objets sont-ils ordonnés ? En d'autres termes, si nous parcourons un objet en boucle, obtenons-nous toutes les propriétés dans le même ordre où elles ont été ajoutées ? Pouvons-nous compter sur cela ?

La réponse courte est : "ordonné de manière spéciale" : les propriétés des entiers sont triées, les autres apparaissent dans l'ordre de création. Nous allons voir cela en détails.

Par exemple, considérons un objet avec les indicatifs de téléphone par pays :

```js run
let codes = {
  "49": "Germany",
  "41": "Switzerland",
  "44": "Great Britain",
  // ..,
  "1": "USA"
};

*!*
for(let code in codes) {
  alert(code); // 1, 41, 44, 49
}
*/!*
```

L'objet peut être utilisé pour suggérer une liste d'options à l'utilisateur. Si nous créons un site principalement pour le public allemand, nous voulons probablement que `49` soit le premier.

Mais si nous exécutons ce code, nous voyons une image totalement différente :

- USA (1) passe en premier
- puis Switzerland (41) et ainsi de suite.

Les indicatifs de téléphone sont classés par ordre croissant, car ce sont des entiers. Donc on voit `1, 41, 44, 49`.

````smart header="Propriétés entier (integer properties) ? Qu'est-ce que c'est ?"
Le terme "propriété entier" (integer properties) désigne ici une chaîne de caractères pouvant être convertie en un entier à partir de-et-sans modification.

Ainsi, "49" est un nom de propriété entier, parce que lorsqu'il est transformé en nombre entier et inversement, il reste identique. Mais "+49" et "1.2" ne le sont pas :

```js run
// Math.trunc est une fonction intégrée qui supprime la partie décimale
alert( String(Math.trunc(Number("49"))) ); // "49", idem, propriété entier (integer property)
alert( String(Math.trunc(Number("+49"))) ); // "49", pas pareil "+49" ⇒ pas une propriété entier (integer property)
alert( String(Math.trunc(Number("1.2"))) ); // "1", pas pareil "1.2" ⇒ pas une propriété entier (integer property)
```
````

… Par contre, si les clés ne sont pas des entiers, elles sont listées dans l'ordre de création, par exemple :

```js run
let user = {
  name: "John",
  surname: "Smith"
};
user.age = 25; // Ajouter une clé de plus

*!*
// les propriétés non-entiers sont listées dans l'ordre de création
*/!*
for (let prop in user) {
  alert( prop ); // name, surname, age
}
```

Donc, pour résoudre le problème avec les indicatifs de téléphone, nous pouvons "tricher" en rendant ces indicatifs non entiers. Ajouter un signe plus `"+"` avant chaque indicatif suffit.

Comme ceci :

```js run
let codes = {
  "+49": "Germany",
  "+41": "Switzerland",
  "+44": "Great Britain",
  // ..,
  "+1": "USA"
};

for(let code in codes) {
  alert( +code ); // 49, 41, 44, 1
}
```

Maintenant, cela fonctionne comme prévu.

## Copier par référence

L'une des différences fondamentales entre les objets et les primitives est qu'ils sont stockés et copiés "par référence".

Les valeurs primitives: chaînes de caractères, nombres, booléens - sont attribuées / copiées "comme une seule valeur".

Par exemple :

```js
let message = "Hello!";
let phrase = message;
```

Par conséquent, nous avons deux variables indépendantes, chacune stockant la chaîne de caractères `"Hello!"`.

![](variable-copy-value.svg)

Les objets ne sont pas comme ça.

**Une variable ne stocke pas l'objet lui-même, mais son "adresse en mémoire", autrement dit "une référence".**

Voici l'image d'un objet :

```js
let user = {
  name: "John"
};
```

![](variable-contains-reference.svg)

Ici, l'objet est stocké quelque part en mémoire. Et la variable `user` a une "référence" à cet objet.

**Lorsqu'une variable d'objet est copiée - la référence est copiée, l'objet n'est pas dupliqué.**

Si nous imaginons un objet comme une armoire, une variable en est la clé. La copie d'une variable duplique la clé, mais pas l'armoire elle-même.

Par exemple :

```js no-beautify
let user = { name: "John" };

let admin = user; // copier la référence
```

Maintenant nous avons deux variables, chacune avec la référence au même objet :

![](variable-copy-reference.svg)

Nous pouvons utiliser n’importe quelle variable pour accéder à l'armoire et modifier son contenu :

```js run
let user = { name: 'John' };

let admin = user;

*!*
admin.name = 'Pete'; // modifié par la référence "admin"
*/!*

alert(*!*user.name*/!*); // 'Pete', les changements sont visibles depuis la référence "user"
```

L'exemple ci-dessus montre qu'il n'y a qu'un seul objet. Comme si nous avions une armoire avec deux clés et en utilisions une (`admin`) pour y entrer. Ensuite, si nous utilisions plus tard l'autre clé (`user`), nous constaterions des changements.

### Comparaison par référence

Les opérateurs d'égalité `==` et d'égalité stricte `===` pour les objets fonctionnent exactement de la même manière.

**Deux objets ne sont égaux que s’ils sont le même objet.**

<<<<<<< HEAD
Par exemple, deux variables référencent le même objet, elles sont égales :
=======
For instance, if two variables reference the same object, they are equal:
>>>>>>> 34e9cdca3642882bd36c6733433a503a40c6da74

```js run
let a = {};
let b = a; // copier la référence

alert( a == b ); // true, les deux variables font référence au même objet
alert( a === b ); // true
```

Et ici, deux objets indépendants ne sont pas égaux, même s'ils sont vides :

```js run
let a = {};
let b = {}; // deux objets indépendants

alert( a == b ); // false
```

Pour des comparaisons comme `obj1 > obj2` ou pour une comparaison avec une primitive `obj == 5`, les objets sont convertis en primitives. Nous étudierons très bientôt le fonctionnement des conversions d’objets, mais à vrai dire, de telles comparaisons ne sont nécessaires que très rarement et résultent généralement d’une erreur de codage.

### Objet Const

Un objet déclaré comme `const` *peut* être modifié.

Par exemple :

```js run
const user = {
  name: "John"
};

*!*
user.age = 25; // (*)
*/!*

alert(user.age); // 25
```

<<<<<<< HEAD
Il peut sembler que la ligne `(*)` cause une erreur, mais non, il n’y a absolument aucun problème. C’est parce que `const` fixe la valeur de `user` lui-même. Et ici, `user` stocke la référence au même objet tout le temps. La ligne `(*)` va à l’intérieur de l’objet, elle ne réaffecte pas `user`.
=======
It might seem that the line `(*)` would cause an error, but no, there's totally no problem. That's because `const` fixes only value of `user` itself. And here `user` stores the reference to the same object all the time. The line `(*)` goes *inside* the object, it doesn't reassign `user`.
>>>>>>> 34e9cdca3642882bd36c6733433a503a40c6da74

Le `const` donnerait une erreur si nous essayons de définir `user` sur autre chose, par exemple :

```js run
const user = {
  name: "John"
};

*!*
// Erreur (ne peut pas réaffecter user)
*/!*
user = {
  name: "Pete"
};
```

… Mais que faire si nous voulons créer des propriétés d'objet constantes ? Donc, `user.age = 25` donnerait une erreur. C’est possible aussi. Nous couvrirons cela dans le chapitre <info:property-descriptors>.

## Clonage et fusion, Object.assign

Donc, la copie d'une variable d'objet crée une autre référence au même objet.

Mais que faire si nous avons besoin de dupliquer un objet ? Créer une copie indépendante, un clone ?

C’est également faisable, mais un peu plus difficile, car il n’existe pas de méthode intégrée pour cela en JavaScript. En fait, cela est rarement nécessaire. La copie par référence est bonne la plupart du temps.

Mais si nous le voulons vraiment, nous devons créer un nouvel objet et répliquer la structure de l’existant en itérant sur ses propriétés et en les copiant au niveau primitif.

Comme ceci :

```js run
let user = {
  name: "John",
  age: 30
};

*!*
let clone = {}; // le nouvel objet vide

// copions toutes les propriétés de user dedans
for (let key in user) {
  clone[key] = user[key];
}
*/!*

// maintenant clone est un clone totalement indépendant
clone.name = "Pete"; // changé les données qu'il contient

alert( user.name ); // toujours John dans l'objet original
```

Nous pouvons aussi utiliser la méthode [Object.assign](mdn:js/Object/assign) pour cela.

La syntaxe est :

```js
Object.assign(dest[, src1, src2, src3...])
```

- Arguments `dest`, et `src1, ..., srcN` (peut être autant que nécessaire) sont des objets.
- Il copie les propriétés de tous les objets `src1, ..., srcN` dans `dest`. En d'autres termes, les propriétés de tous les arguments à partir du 2e sont copiées dans le 1er. Puis ça renvoit `dest`.

Par exemple, nous pouvons l’utiliser pour fusionner plusieurs objets en un seul :
```js
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

*!*
// copie toutes les propriétés de permissions1 et permissions2 dans user
Object.assign(user, permissions1, permissions2);
*/!*

// maintenant user = { name: "John", canView: true, canEdit: true }
```

Si l'objet destinataire (`user`) a déjà la même propriété nommée, elle sera écrasée :

```js
let user = { name: "John" };

// écrase name, ajoute isAdmin
Object.assign(user, { name: "Pete", isAdmin: true });

// maintenant user = { name: "Pete", isAdmin: true }
```

Nous pouvons également utiliser `Object.assign` pour remplacer la boucle pour un clonage simple :

```js
let user = {
  name: "John",
  age: 30
};

*!*
let clone = Object.assign({}, user);
*/!*
```

Il copie toutes les propriétés de `user` dans l'objet vide et le renvoie. En fait, c'est le même principe que la boucle, mais  en plus court.

Jusqu'à présent, nous avons supposé que toutes les propriétés de `user` sont primitives. Mais les propriétés peuvent être des références à d'autres objets. Que faire avec elles ?

Comme ceci :
```js run
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

alert( user.sizes.height ); // 182
```

Maintenant, il ne suffit plus de copier `clone.sizes = user.sizes`, car `user.sizes` est un objet, il sera copié par référence. Donc, `clone` et `user` partageront les mêmes tailles :

Comme ceci :
```js run
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50
  }
};

let clone = Object.assign({}, user);

alert( user.sizes === clone.sizes ); // true, même objet

// user et clone partagent les mêmes tailles
user.sizes.width++;       // change une propriété d'un endroit
alert(clone.sizes.width); // 51, voir le résultat de l'autre endroit
```

Pour résoudre ce problème, nous devons utiliser la boucle de clonage qui examine chaque valeur de `user[key]` et, s’il s’agit d’un objet, répliquer également sa structure. Cela s'appelle un "clonage en profondeur" (deep cloning).

Il existe un algorithme standard pour le clonage en profondeur qui traite le cas ci-dessus et des cas plus complexes, appelé l'[Algorithme de clonage structuré](http://w3c.github.io/html/infrastructure.html#safe-passing-of-structured-data). Pour ne pas réinventer la roue, nous pouvons utiliser une implémentation fonctionnelle de celle-ci à partir de la bibliothèque JavaScript [lodash](https://lodash.com), la méthode s'appelle [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep).



## Résumé

Les objets sont des tableaux associatifs dotés de plusieurs fonctionnalités spéciales.

Ils stockent des propriétés (paires clé-valeur), où :
- Les clés de propriété doivent être des chaînes de caractères ou des symboles (généralement des chaînes de caractères).
- Les valeurs peuvent être de tout type.

Pour accéder à une propriété, nous pouvons utiliser :
- La notation par points : `obj.property`.
- Notation entre crochets `obj["property"]`. Les crochets permettent de prendre la clé d’une variable, comme `obj[varWithKey]`.

Opérateurs supplémentaires :
- Pour supprimer une propriété : `delete obj.prop`.
- Pour vérifier si une propriété avec la clé donnée existe : `"key" in obj`.
- Pour parcourir un objet la boucle : `for(let key in obj)`.

Les objets sont assignés et copiés par référence. En d'autres termes, une variable stocke non pas la "valeur d'objet", mais une "référence" (adresse en mémoire) pour la valeur. Donc, copier une telle variable ou la passer comme argument de fonction copie cette référence, pas l'objet. Toutes les opérations via des références copiées (telles que l'ajout / la suppression de propriétés) sont effectuées sur le même objet.

Pour faire une "copie réelle" (un clone), nous pouvons utiliser `Object.assign` ou  [_.cloneDeep(obj)](https://lodash.com/docs#cloneDeep).

Ce que nous avons étudié dans ce chapitre s’appelle un "objet simple" (plain object) ou juste `Object`.  
Il existe de nombreux autres types d'objets en JavaScript :

- `Array` pour stocker des collections de données ordonnées,
- `Date` pour stocker des informations sur la date et l'heure,
- `Error` pour stocker des informations sur une erreur.
- … Etc.

Ils ont leurs particularités que nous étudierons plus tard. Parfois, les gens disent quelque chose comme "Type de tableau" ou "Type de date", mais ils ne sont pas formellement propres, mais appartiennent à un seul type de données "objet". Et ils l'étendent de différentes manières.

Les objets en JavaScript sont très puissants. Nous venons de gratter la surface d’un sujet vraiment énorme. Nous allons travailler étroitement avec les objets et en apprendre davantage à leur sujet dans d’autres parties du tutoriel.
