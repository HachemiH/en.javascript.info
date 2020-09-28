# L'opérateur de coalescence des nuls '??'

[recent browser="new"]

<<<<<<< HEAD
L'opérateur de coalescence des nuls `??` fournit une syntaxe courte pour sélectionner une première variable "définie" dans la liste.

Le résultat de `a ?? b` est:
- `a` si ce n'est pas `null` ou `undefined`,
- `b`, sinon.

Donc, `x = a ?? b` est un équivalent court de :
=======
Here, in this article, we'll say that an expression is "defined" when it's neither `null` nor `undefined`.

The nullish coalescing operator is written as two question marks `??`.

The result of `a ?? b` is:
- if `a` is defined, then `a`,
- if `a` isn't defined, then `b`.


In other words, `??` returns the first argument if it's defined. Otherwise, the second one.

The nullish coalescing operator isn't anything completely new. It's just a nice syntax to get the first "defined" value of the two.

We can rewrite `result = a ?? b` using the operators that we already know, like this:
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d

```js
result = (a !== null && a !== undefined) ? a : b;
```

The common use case for `??` is to provide a default value for a potentially undefined variable.

For example, here we show `Anonymous` if `user` isn't defined:

```js run
let user;

alert(user ?? "Anonymous"); // Anonymous
```

<<<<<<< HEAD
Voici un exemple plus long.


Imaginez, nous avons un utilisateur, et il y a des variables `firstName`, `lastName` ou `nickName` pour leur prénom, nom et surnom. Tous peuvent être indéfinis, si l'utilisateur décide de ne saisir aucune valeur.

Nous aimerions afficher le nom d'utilisateur : l'une de ces trois variables, ou afficher "Anonymous" si rien n'est défini.

Utilisons l'opérateur `??` pour sélectionner le premier défini :
=======
Of course, if `user` had any value except `null/undefined`, then we would see it instead:

```js run
let user = "John";

alert(user ?? "Anonymous"); // John
```

We can also use a sequence of `??` to select the first defined value from a list.

Let's say we have a user's data in variables `firstName`, `lastName` or `nickName`. All of them may be undefined, if the user decided not to enter a value.

We'd like to display the user name using one of these variables, or show "Anonymous" if all of them are undefined.

Let's use the `??` operator for that:
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d

```js run
let firstName = null;
let lastName = null;
let nickName = "Supercoder";

<<<<<<< HEAD
// Montre la première variable qui ne soit ni null, ni undefined
=======
// shows the first defined value:
*!*
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d
alert(firstName ?? lastName ?? nickName ?? "Anonymous"); // Supercoder
```

## Comparaison avec ||

<<<<<<< HEAD
L'opérateur OR `||` peut être utilisé de la même manière que `??`. En fait, nous pouvons remplacer `??` par `||` dans le code ci-dessus et obtenir le même résultat, comme il a été décrit dans le [chapitre précédent](info:logical-operators#or-finds-the-first-truthy-value).

La différence importante est que :
- `||` renvoie la première valeur *vraie*.
- `??` renvoie la première valeur *définie*.

Cela importe beaucoup lorsque nous souhaitons traiter `null/undefined` différemment de `0`.

Par exemple :
=======
The OR `||` operator can be used in the same way as `??`, as it was described in the [previous chapter](info:logical-operators#or-finds-the-first-truthy-value).

For example, in the code above we could replace `??` with `||` and still get the same result:

```js run
let firstName = null;
let lastName = null;
let nickName = "Supercoder";
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d

// shows the first truthy value:
*!*
alert(firstName || lastName || nickName || "Anonymous"); // Supercoder
*/!*
```

<<<<<<< HEAD
Cela définit `height` à `100` si elle n'est pas définie. 

Comparons-le avec `||` :
=======
The OR `||` operator exists since the beginning of JavaScript, so developers were using it for such purposes for a long time.

On the other hand, the nullish coalescing operator `??` was added only recently, and the reason for that was that people weren't quite happy with `||`.

The subtle, yet important difference is that:
- `||` returns the first *truthy* value.
- `??` returns the first *defined* value.

In other words, `||` doesn't distinguish between `false`, `0`, an empty string `""` and `null/undefined`. They are all the same -- falsy values. If any of these is the first argument of `||`, then we'll get the second argument as the result.

In practice though, we may want to use default value only when the variable is `null/undefined`. That is, when the value is really unknown/not set.

For example, consider this:
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d

```js run
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

<<<<<<< HEAD
Ici, `height || 100` traite la hauteur zéro comme non définie, identique à `null`, `undefined` ou toute autre valeur fausse. Donc le résultat est `100`.

L'opération `height ?? 100` renvoie `100` uniquement si `height` est exactement `null` ou `undefined`. Ainsi, l'alerte affiche la valeur de hauteur `0` "telle quelle".

Le meilleur comportement dépend d'un cas d'utilisation particulier. Lorsque la hauteur zéro est une valeur valide, alors `??` est préférable.

## Priorité
=======
Here, we have a zero height.

- The `height || 100` checks `height` for being a falsy value, and it really is.
    - so the result is the second argument, `100`.
- The `height ?? 100` checks `height` for being `null/undefined`, and it's not,
    - so the result is `height` "as is", that is `0`.

If we assume that zero height is a valid value, that shouldn't be replaced with the default, then `??` does just the right thing.
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d

La priorité de l'opérateur `??` est plutôt faible : `5` dans le [tableau MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#Table).

<<<<<<< HEAD
Ainsi, `??` est évalué après la plupart des autres opérations, mais avant `=` et `?`.


Si nous devons choisir une valeur avec `??` dans une expression complexe, envisagez d'ajouter des parenthèses :
=======
The precedence of the `??` operator is rather low: `5` in the [MDN table](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#Table). So `??` is evaluated before `=` and `?`, but after most other operations, such as `+`, `*`.

So if we'd like to choose a value with `??` in an expression with other operators, consider adding parentheses:
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d

```js run
let height = null;
let width = null;

// important : utilisez des parenthèses
let area = (height ?? 100) * (width ?? 50);

alert(area); // 5000
```

<<<<<<< HEAD
Sinon, si nous omettons les parenthèses, `*` a la priorité plus élevée que `??` et s'exécuterait en premier.

Cela fonctionnerait comme :

```js
// probablement pas correct
let area = height ?? (100 * width) ?? 50;
```

Il existe également une limitation au niveau du language.

**Pour des raisons de sécurité, il est interdit d'utiliser `??` avec les opérateurs `&&` et `||`.**
=======
Otherwise, if we omit parentheses, then as `*` has the higher precedence than `??`, it would execute first, leading to incorrect results.

```js
// without parentheses
let area = height ?? 100 * width ?? 50;

// ...works the same as this (probably not what we want):
let area = height ?? (100 * width) ?? 50;
```

### Using ?? with && or ||

Due to safety reasons, JavaScript forbids using `??` together with `&&` and `||` operators, unless the precedence is explicitly specified with parentheses.
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d

Le code ci-dessous déclenche une erreur de syntaxe :

```js run
let x = 1 && 2 ?? 3; // Syntax error
```

<<<<<<< HEAD
La limitation est sûrement discutable, mais elle a été ajoutée à la spécification du langage dans le but d'éviter les erreurs de programmation, car les gens commencent à passer à `??` à partir de `||`.
=======
The limitation is surely debatable, but it was added to the language specification with the purpose to avoid programming mistakes, when people start to switch to `??` from `||`.
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d

Utilisez des parenthèses explicites pour la contourner :

```js run
*!*
let x = (1 && 2) ?? 3; // fonctionne
*/!*

alert(x); // 2
```

## Résumé

- L'opérateur de coalescence des nuls `??` fournit un moyen court de choisir une valeur "définie" dans la liste.

    Il est utilisé pour attribuer des valeurs par défaut aux variables :

    ```js
    // configurer height=100, si height est null ou undefined
    height = height ?? 100;
    ```

<<<<<<< HEAD
- L'opérateur `??` a une priorité très faible, un peu plus élevée que `?` Et `=`.
- Il est interdit de l'utiliser avec `||` ou `&&` sans parenthèses explicites.
=======
- The operator `??` has a very low precedence, a bit higher than `?` and `=`, so consider adding parentheses when using it in an expression.
- It's forbidden to use it with `||` or `&&` without explicit parentheses.
>>>>>>> f489145731a45df6e369a3c063e52250f3f0061d
