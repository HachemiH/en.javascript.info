

# Chaînage optionnel '?.'

[recent browser="new"]


Le chaînage optionnel `?.` Est un moyen sécurisé d'accéder aux propriétés d'objet imbriquées, même si une propriété intermédiaire n'existe pas.

## Le problème de la "propriété non existante"


Si vous venez de commencer à lire le tutoriel et à apprendre JavaScript, peut-être que le problème ne vous a pas encore touché, mais c'est assez courant.


À titre d'exemple, disons que nous avons des objets `user` qui contiennent les informations sur nos utilisateurs.

La plupart de nos utilisateurs ont des adresses dans la propriété `user.address`, avec la rue `user.address.street`, mais certains ne les ont pas fournies.

Dans ce cas, lorsque nous essayons d'obtenir `user.address.street`, et que l'utilisateur se trouve sans adresse, nous obtenons une erreur :

```js run
let user = {}; // un utilisateur sans propriété "address"


alert(user.address.street); // Error!
```

C'est le résultat attendu. JavaScript fonctionne comme cela. Comme `user.address` est `undefined`, une tentative d'obtention de `user.address.street` échoue avec une erreur.

Dans de nombreux cas pratiques, nous préférerions obtenir `undefined` au lieu d'une erreur ici (signifiant "pas de rue").

... Et un autre exemple. Dans le développement Web, nous pouvons obtenir un objet qui correspond à un élément de page Web à l'aide d'un appel de méthode spécial, tel que `document.querySelector('.elem')`, et il renvoie `null` lorsqu'il n'y a pas ce type d'élément.

```js run
// document.querySelector('.elem') est nul s'il n'y a pas d'élément
let html = document.querySelector('.elem').innerHTML; // error if it's null
```

Encore une fois, si l'élément n'existe pas, nous obtiendrons une erreur lors de l'accès à la propriété `.innerHTML` de` null`. Et dans certains cas, lorsque l'absence de l'élément est normale, nous aimerions éviter l'erreur et accepter simplement `html = null` comme résultat.

Comment peut-on le faire ?

La solution évidente serait de vérifier la valeur en utilisant `if` ou l'opérateur conditionnel `?`, Avant d'accéder à sa propriété, comme ceci :

```js
let user = {};

alert(user.address ? user.address.street : undefined);
```

Cela fonctionne, il n'y a pas d'erreur ... Mais c'est assez inélégant. Comme vous pouvez le voir, `"user.address"` apparaît deux fois dans le code. 


Voici à quoi ressemblerait la même chose pour `document.querySelector` :

```js run
let html = document.querySelector('.elem') ? document.querySelector('.elem').innerHTML : null;
```

Nous pouvons voir que l'élément de recherche `document.querySelector('.elem')` est en fait appelé deux fois ici. Pas bon.

Pour les propriétés plus profondément imbriquées, cela devient encore plus laid, car davantage de répétitions sont nécessaires.

Par exemple. récupérons `user.address.street.name` de la même manière.

```js
let user = {}; // l'utilisateur n'a pas d'adresse

alert(user.address ? user.address.street ? user.address.street.name : null : null);
```

C'est juste horrible, on peut même avoir des problèmes pour comprendre un tel code.

Il existe une meilleure façon de l'écrire, en utilisant l'opérateur `&&` :

```js run
let user = {}; // l'utilisateur n'a pas d'adresse

alert( user.address && user.address.street && user.address.street.name ); // undefined (no error)
```

Et le chemin complet vers la propriété garantit que tous les composants existent (sinon, l'évaluation s'arrête), mais n'est pas non plus idéal.

Comme vous pouvez le voir, les noms de propriétés sont toujours dupliqués dans le code. Par exemple. dans le code ci-dessus, `user.address` apparaît trois fois.

C'est pourquoi le chaînage facultatif `?.` A été ajouté au langage. Pour résoudre ce problème une fois pour toutes !


## Chaînage optionnel

Le chaînage optionnel `?.` arrête l'évaluation si la valeur avant `?.` est `undefined` ou `null` et renvoie `undefined`.

**Plus loin dans cet article, par souci de brièveté, nous dirons que quelque chose "existe" si ce n'est pas "null" et non "undefined".**

En d'autres termes, `value?.prop` :
- est identique à `value.prop` si `value` existe,
- sinon (lorsque `value` est `undefined/nul`), il renvoie `undefined`.

Voici le moyen sûr d'accéder à `user.address.street` en utilisant `?.` :

```js run
let user = {}; // l'utilisateur n'a pas d'adresse

alert( user?.address?.street ); // undefined (no error)
```

Le code est court et propre, il n'y a aucune duplication.

Voici un exemple avec `document.querySelector` :

```js run
let html = document.querySelector('.elem')?.innerHTML; // will be undefined, if there's no element
```

La lecture de l'adresse avec `user?.address` fonctionne même si l'objet `user` n'existe pas :

```js run
let user = null;

alert( user?.address ); // undefined
alert( user?.address.street ); // undefined
```

Remarque: la syntaxe `?.` Rend facultative la valeur qui la précède, mais pas plus.

Par exemple. dans `user?.address.street.name` le `?.` permet à `user` d'être en toute sécurité `null/undefined` (et renvoie `undefined` dans ce cas), mais ce n'est que pour `user`. D'autres propriétés sont accessibles de manière régulière. Si nous voulons que certaines d'entre elles soient optionnelles, alors nous devrons remplacer plus de `.` par `?.`.

```warn header="N'abusez pas du chaînage optionnel"
Nous ne devrions utiliser `?.` que là où il est normal que quelque chose n'existe pas.

Par exemple, si selon notre logique de codage, l'objet `user` doit exister, mais que `address` est facultatif, alors nous devrions écrire `user.address?.street`, mais pas `user?.address?.street`.

Ensuite, si `user` n'est pas défini, nous verrons une erreur de programmation à ce sujet et nous la corrigerons. Sinon, si nous abusons de `?.`, les erreurs de codage peuvent être réduites au silence là où cela n'est pas approprié et devenir plus difficiles à déboguer.
```

````warn header="La variable avant `?.` doit être déclarée"
S'il n'y a pas du tout de variable `user`, alors `user?.anything` déclenche une erreur :

```js run
// ReferenceError: user is not defined
user?.address;
```
La variable doit être déclarée (par exemple `let/const/var user` ou en tant que paramètre de fonction). Le chaînage facultatif ne fonctionne que pour les variables déclarées.
````

## Court-circuit

Comme il a été dit précédemment, le `?.` arrête immédiatement ("court-circuite") l'évaluation si la partie gauche n'existe pas.


Ainsi, s'il y a d'autres appels de fonction ou opérations à droite de `?.`, elles ne seront pas effectuées.

Par exemple :

```js run
let user = null;
let x = 0;

user?.sayHi(x++); // pas de "user", donc l'exécution n'atteint pas l'appel sayHi et x++


alert(x); // 0, la valeur n'est pas incrémenté
```


## Autres variantes : ?.(), ?.[]

`?.` n'est pas un opérateur, mais une construction syntaxique particulière qui fonctionne aussi avec les appels de fonction et les crochets.

Par exemple, `?.()` est utilisé pour exécuter une fonction seulement si elle existe.

Ainsi dans cet exemple la méthode `admin` n'existe pas pour tout le monde :

```js run
let userAdmin = {
  admin() {
    alert("I am admin");
  }
};

let userGuest = {};

*!*
userAdmin.admin?.(); // I am admin
*/!*

*!*
userGuest.admin?.(); // nothing happens (no such method)
*/!*
```

Ici, dans les deux lignes, nous utilisons d'abord le point (`userAdmin.admin`) pour obtenir la propriété `admin`, car nous supposons que l'objet `user` existe, il peut donc être lu en toute sécurité.

<<<<<<< HEAD
Puis `?.()` Vérifie la partie gauche : si la fonction admin existe, alors elle s'exécute (c'est le cas pour `userAdmin`). Sinon (pour `userGuest`) l'évaluation s'arrête sans erreur.
=======
Then `?.()` checks the left part: if the `admin` function exists, then it runs (that's so for `userAdmin`). Otherwise (for `userGuest`) the evaluation stops without errors.
>>>>>>> 291b5c05b99452cf8a0d32bd32426926dbcc0ce0


La syntaxe `?.[]` Fonctionne également, si nous voulons utiliser des crochets `[]` pour accéder aux propriétés au lieu du point `.`. Similaire aux cas précédents, il permet de lire en toute sécurité une propriété à partir d'un objet qui peut ne pas exister.

```js run
let key = "firstName";

let user1 = {
  firstName: "John"
};

let user2 = null;

alert( user1?.[key] ); // John
alert( user2?.[key] ); // undefined
```

Nous pouvons également utiliser `?.` avec `delete` :

```js run
delete user?.name; // supprime user.name si user existe
```

<<<<<<< HEAD

```warn header="Nous pouvons utiliser `?.` pour lire et supprimer en toute sécurité, mais pas pour écrire"
Le chaînage optionnel `?.` n'a aucune utilité sur le côté gauche d'une affectation :

=======
````warn header="We can use `?.` for safe reading and deleting, but not writing"
The optional chaining `?.` has no use on the left side of an assignment.
>>>>>>> 291b5c05b99452cf8a0d32bd32426926dbcc0ce0

For example:
```js run

let user = null;

user?.name = "John"; // Erreur, ne fonctionne pas
// car il évalue : undefined = "John"
```
Ce n'est tout simplement pas si intelligent.

## Résumé

Le chaînage optionnel '?.' A trois formes :

1. `obj?.prop` -- retourne `obj.prop` si `obj` existe, sinon `undefined`.
2. `obj?.[prop]` -- retourne `obj[prop]` si `obj` existe, sinon `undefined`.
3. `obj?.method()` -- appel `obj.method()` si `obj.method` existe, sinon retourne `undefined`.


Comme nous pouvons le voir, tous sont simples et simples à utiliser. Le `?.` vérifie la partie gauche pour `nul/undefined` et permet à l'évaluation de se poursuivre si ce n'est pas le cas.

Une chaîne de `?.` permet d'accéder en toute sécurité aux propriétés imbriquées.


Néanmoins, nous devons appliquer `?.` avec précaution, uniquement là où il est acceptable, selon la logique de notre code, que la partie gauche n'existe pas. Pour qu'il ne nous cache pas les erreurs de programmation, si elles se produisent.
