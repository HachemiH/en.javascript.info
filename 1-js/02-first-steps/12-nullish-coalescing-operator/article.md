# L'opérateur de coalescence des nuls '??'

[recent browser="new"]

L'opérateur de coalescence des nuls est écrit sous la forme de deux points d'interrogation `??`.

Comme il traite `null` et `undefined` de la même manière, nous utiliserons un terme spécial ici, dans cet article. Par souci de brièveté, nous dirons qu'une expression est "définie" lorsqu'elle n'est ni `null` ni `undefined`.

Le résultat de `a ?? b` est :
- si `a` est défini, alors `a`,
- si `a` n'est pas défini, alors `b`.

En d'autres termes, `??` renvoie le premier argument s'il n'est pas `null`/`undefined`. Sinon, le second.

L'opérateur de coalescence des nuls n'est pas complètement nouveau. C'est juste une belle syntaxe pour obtenir la première valeur "defined" des deux.

Nous pouvons réécrire `result = a ?? b` en utilisant les opérateurs que nous connaissons déjà, comme ceci :

```js
result = (a !== null && a !== undefined) ? a : b;
```

Maintenant, il devrait être absolument clair ce que fait `??`. Voyons où cela aide.

Le cas d'utilisation courant de `??` est de fournir une valeur par défaut.

Par exemple, nous affichons ici `user` si sa valeur n'est pas `null`/`undefined`, sinon `Anonymous` :

```js run
let user;

alert(user ?? "Anonymous"); // Anonymous (user is undefined)
```

Voici l'exemple avec `user` attribué à un nom :

```js run
let user = "John";

alert(user ?? "Anonymous"); // John (user is not null/undefined)
```

Nous pouvons également utiliser une séquence de `??` pour sélectionner la première valeur dans une liste qui n'est pas `null`/`undefined`.

Disons que nous avons les données d'un utilisateur dans les variables `firstName`, `lastName` ou `nickName`. Tous peuvent être indéfinis, si l'utilisateur décide de ne pas entrer de valeurs correspondantes.

Nous aimerions afficher le nom d'utilisateur à l'aide de l'une de ces variables, ou afficher "Anonyme" si toutes sont `null`/`undefined`.

Utilisons l'opérateur `??` pour cela :

```js run
let firstName = null;
let lastName = null;
let nickName = "Supercoder";

// affiche la première valeur définie :
*!*
alert(firstName ?? lastName ?? nickName ?? "Anonymous"); // Supercoder
*/!*
```

## Comparaison avec ||

L'opérateur OR `||` peut être utilisé de la même manière que `??`, comme il a été décrit dans le [chapitre précédent](info:logical-operators#or-finds-the-first-truthy-value).

Par exemple, dans le code ci-dessus, nous pourrions remplacer `??` par `||` et toujours obtenir le même résultat :

```js run
let firstName = null;
let lastName = null;
let nickName = "Supercoder";

// affiche la première valeur vraie :
*!*
alert(firstName || lastName || nickName || "Anonymous"); // Supercoder
*/!*
```

<<<<<<< HEAD
Historiquement, l'opérateur OR `||` était là en premier. Il existe depuis le début de JavaScript, donc les développeurs l'utilisaient à de telles fins depuis longtemps.
=======
Historically, the OR `||` operator was there first. It's been there since the beginning of JavaScript, so developers were using it for such purposes for a long time.
>>>>>>> 285083fc71ee3a7cf55fd8acac9c91ac6f62105c

D'un autre côté, l'opérateur de coalescence des nuls `??` n'a été ajouté à JavaScript que récemment, et la raison en était que les gens n'étaient pas tout à fait satisfaits de `||`.

La différence importante entre eux est que :
- `||` renvoie la première valeur *vraie*.
- `??` renvoie la première valeur *définie*.

En d'autres termes, `||` ne fait pas la distinction entre `false`, `0`, une chaîne vide `" "` et `null`/`undefined`. Ce sont tous les mêmes -- des valeurs fausses. Si l'un de ceux-ci est le premier argument de `||`, alors nous obtiendrons le deuxième argument comme résultat.

Dans la pratique cependant, nous pouvons vouloir utiliser la valeur par défaut uniquement lorsque la variable est `null`/`undefined`. Autrement dit, lorsque la valeur est vraiment inconnue/non définie.

Par exemple, considérez ceci :

```js run
let height = 0;

alert(height || 100); // 100
alert(height ?? 100); // 0
```

- L'expression `height || 100` vérifie que `height` est une valeur fausse, et c'est `0`, elle est fausse en effet.
    - donc le résultat de `||` est le deuxième argument, `100`.
- L'expression `height ?? 100` vérifie que `height` est `null`/`undefined`, et ce n'est pas le cas,
    - donc le résultat est `height` "tel quel", c'est-à-dire `0`.

En pratique, la hauteur zéro est souvent une valeur valide, qui ne doit pas être remplacée par la valeur par défaut. Alors `??` fait ce qu'il faut.

## Priorité

La priorité de l'opérateur `??` est la même que celle de `||`. Elle est égale à `3` dans le [tableau MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence#Table).

Cela signifie que, tout comme `||`, l'opérateur de coalescence des nuls `??` est évalué avant `=` et `?`, mais après la plupart des autres opérations, telles que `+`, `*`.

Nous devrons donc peut-être ajouter des parenthèses dans des expressions comme celle-ci :

```js run
let height = null;
let width = null;

// important : utilisez des parenthèses
let area = (height ?? 100) * (width ?? 50);

alert(area); // 5000
```

Sinon, si nous omettons les parenthèses, alors que `*` a une priorité plus élevée que `??`, il s'exécuterait en premier, conduisant à des résultats incorrects.

```js
// sans parenthèses
let area = height ?? 100 * width ?? 50;

// ...fonctionne de cette façon (pas ce que nous voulons) :
let area = height ?? (100 * width) ?? 50;
```

### En utilisant ?? avec && ou ||

Pour des raisons de sécurité, JavaScript interdit d'utiliser `??` avec les opérateurs `&&` et `||`, à moins que la priorité ne soit explicitement spécifiée entre parenthèses.

Le code ci-dessous déclenche une erreur de syntaxe :

```js run
let x = 1 && 2 ?? 3; // Syntax error
```

La limitation est sûrement discutable, mais elle a été ajoutée à la spécification du langage dans le but d'éviter les erreurs de programmation, quand les gens commencent à passer de `||` à `??`.

Utilisez des parenthèses explicites pour la contourner :

```js run
*!*
let x = (1 && 2) ?? 3; // fonctionne
*/!*

alert(x); // 2
```

## Résumé

- L'opérateur de coalescence des nuls `??` fournit un moyen court de choisir une valeur "définie" à partir d'une liste.

    Il est utilisé pour attribuer des valeurs par défaut aux variables :

    ```js
    // configurer height = 100, si height est null ou undefined
    height = height ?? 100;
    ```

- L'opérateur `??` a une priorité très faible, un peu plus élevée que `?` et `=`, pensez donc à ajouter des parenthèses lors de son utilisation dans une expression.
- Il est interdit de l'utiliser avec `||` ou `&&` sans parenthèses explicites.
