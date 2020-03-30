libs:
  - lodash

---

# Curryfication

La [curryfication](https://fr.wikipedia.org/wiki/Curryfication) est une technique avancée de travail avec les fonctions. Ce n'est pas seulement utilisé avec JavaScript, mais dans d'autres langages également.

La curryfication est la transformation de fonctions qui traduit une fonction de la forme `f(a, b, c)` en une fonction de la forme `f(a)(b)(c)`.

La curryfication n'est pas une fonction. Elle la transforme simplement.

Voyons d'abord un exemple, pour mieux comprendre de quoi nous parlons, et ensuite mettons en pratique.

Nous allons créer une fonction d'aide `curry(f)` qui curryfie une fonction `f` à deux arguments. En d'autres mots, `curry(f)` sur une fonction `f(a, b)` la traduit en une fonction qui s'appelle par `f(a)(b)` :

```js run
*!*
function curry(f) { // curry(f) fait la curryfication
  return function(a) {
    return function(b) {
      return f(a, b);
    };
  };
}
*/!*

// usage
function sum(a, b) {
  return a + b;
}

let curriedSum = curry(sum);

alert( curriedSum(1)(2) ); // 3
```

Comme vous pouvez le voir, l'implémentation est simple : il ne s'agit que de deux enveloppes.

- Le résultat de `curry(func)` est une enveloppe `function(a)`.
- Lorsqu'il est appelé comme `curriedSum(1)`, l'argument est sauvegardé dans l'environnement lexical, et une nouvelle enveloppe `function(b)` est retournée.
- Ensuite cette enveloppe est appelée avec `2` comme argument, et passe l'appel à la fonction originelle `sum`.

Des implémentations plus avancées de la curryfication, comme [_.curry](https://lodash.com/docs#curry) de la bibliothèque lodash, retournent une enveloppe qui permet à une fonction d'être à la fois appelée normalement et partiellement :

```js run
function sum(a, b) {
  return a + b;
}

let curriedSum = _.curry(sum); // usage de _.curry de la bibliothèque lodash

alert( curriedSum(1, 2) ); // 3, toujours appelable normalement
alert( curriedSum(1)(2) ); // 3, appelée partiellement
```

## La curryfication ? Pour quoi faire ?

Pour comprendre les bénéfices, nous avons besoin d'un exemple réel.

Par exemple, nous avons la fonction de journalisation `log(date, importance, message)` qui formate et écrit l'information. Dans de réels projets de telles fonctions ont beaucoup de fonctionnalités utiles comme envoyer les journaux sur un réseau, ici nous allons juste utiliser `alert` :

```js
function log(date, importance, message) {
  alert(`[${date.getHours()}:${date.getMinutes()}] [${importance}] ${message}`);
}
```

Curryfions-la !

```js
log = _.curry(log);
```

Après ça `log` fonctionne normalement:

```js
log(new Date(), "DEBUG", "some debug"); // log(a, b, c)
```

...Mais aussi dans la forme curryfiée :

```js
log(new Date())("DEBUG")("some debug"); // log(a)(b)(c)
```

Nous pouvons maintenant faire une fonction pratique pour la journalisation actuelle :

```js
// logNow sera la partie partielle de log avec un premier argument fixe
let logNow = log(new Date());

// utilisons-la
logNow("INFO", "message"); // [HH:mm] INFO message
```

Maintenant `logNow` est `log` avec un premier argument fixe, en d'autres termes "fonction partiellement appliquée" ou "partielle" pour faire court.

Nous pouvons aller plus loin et faire une fonction pratique pour le débogage actuel :

```js
let debugNow = logNow("DEBUG");

debugNow("message"); // [HH:mm] DEBUG message
```

Donc :
1. Nous n'avons rien perdu après avoir curryfié : `log` est toujours appelable normalement.
2. Nous pouvons aisément créer des fonctions partielles comme pour la journalisation d'aujourd'hui.

## Implémentation avancée de la curryfication

Au vas où vous souhaitiez entrer dans les détails, voici l'implémentation "avancée" de la curryfication pour les fonctions à plusieurs arguments que nous avons pu utiliser plus haut.

C'est plutôt court :

```js
function curry(func) {

  return function curried(...args) {
    if (args.length >= func.length) {
      return func.apply(this, args);
    } else {
      return function(...args2) {
        return curried.apply(this, args.concat(args2));
      }
    }
  };

}
```

Exemples d'usage :

```js
function sum(a, b, c) {
  return a + b + c;
}

let curriedSum = curry(sum);

alert( curriedSum(1, 2, 3) ); // 6, toujours appelable normalement
alert( curriedSum(1)(2,3) ); // 6, curryfiée au premier argument
alert( curriedSum(1)(2)(3) ); // 6, curryfiée totalement
```

La nouvelle `curry` semble être compliquée, mais est assez simple à comprendre.

Le résultat de `curry(func)` est l'enveloppe `curried` qui ressemble à ça :

```js
// func est la fonction à transformer
function curried(...args) {
  if (args.length >= func.length) { // (1)
    return func.apply(this, args);
  } else {
    return function pass(...args2) { // (2)
      return curried.apply(this, args.concat(args2));
    }
  }
};
```

Quand on la lance, il y a deux branches `if` :

1. Appeler maintenant : si la longueur du `args` donné est supérieure ou égale à la fonction originelle a dans sa déclaration (`func.length`), alors simplement appeler la fonction avec.
2. Obtenir une partielle : sinon, `func` n'est pas encore appelée. À la place, une autre enveloppe `pass` est retournée, qui réappliquera `curried` sur les anciens arguments avec les nouveaux. Enfin sur un nouvel appel, encore, nous obtiendrons une nouvelle partielle (s'il n'y a pas assez d'arguments) ou, enfin, le résultat.

Par exemple, voyons ce qu'il se passe dans le cas de `sum(a, b, c)`. Trois arguments, donc `sum.length = 3`.

Pour l'appel `curried(1)(2)(3)` :

1. Le premier appel `curried(1)` retient `1` dans son environnement lexical, et retourne une enveloppe `pass`.
2. L'enveloppe `pass` est appelée avec `(2)` : elle prend les arguments précédents (`1`), les concatène avec ce qu'elle a (`2`) et appelle `curried(1, 2)` avec lesdits arguments. Comme le nombre d'arguments est toujours inférieur à 3, `curry` retourne `pass`.
3. L'enveloppe `pass` est de nouveau appelée avec `(3)`,  cet appel `pass(3)` prend les anciens arguments (`1`, `2`) et y ajoute `3`, créant l'appel `curried(1, 2, 3)` -- il y a au moins `3` arguments, ils sont donnés à la fonction originelle.

Si ce n'est toujours pas évident, tracez les appels dans votre esprit ou sur un papier.

```smart header="Fonctions à nombre d'arguments fixe seulement"
La curryfaction requiert que la fonction ait un nombre d'arguments fixe.

Une fonction qui utilise un paramètre de reste, comme `f(...args)`, ne peut pas être curryfiée de cette façon.
```

```smart header="Un peu plus que la curryfication"
Par définition, la curryfication devrait convertir `sum(a, b, c)` en `sum(a)(b)(c)`.

Mais la plupart des implémentations en JavaScript sont avancées, comme décrites : elles laissent la possibilité d'appeler la fonction via plusieurs arguments.
```

## Résumé

La *curryfication* est une transformation qui rend `f(a,b,c)` appelable comme `f(a)(b)(c)`. Les implémentations en JavaScript laissent généralement la possibilité d'appeler les fonctions normalement et de retourner une partielle si le nombre d'arguments n'est pas suffisant.

La curryfication nous permet d'avoir aisément des partielles. Comme nous avons pu le voir dans l'exemple de journalisation, après avoir curryfié la fonction à trois arguments, `log(date, importance, message)` nous donne une partielle quand appelée avec un argument (comme `log(date)`) ou deux arguments (comme `log(date, importance)`).
