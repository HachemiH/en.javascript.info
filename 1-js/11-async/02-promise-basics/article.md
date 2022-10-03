# Promesse (promise)

Imaginez que vous êtes un grand chanteur et les fans vous demandent jour et nuit votre prochaine chanson.

Pour avoir un peu de paix, vous promettez de leur envoyer dès que celui-ci est publié. Vous donnez à vos fans une liste d'abonnement. Ils peuvent y ajouter leur adresse mail, comme cela, quand le single est sorti, tous les emails reçoivent votre single. Et même si quelque chose arrive, comme un feu dans le studio, et que vous ne pouvez sortir le single, ils en seront aussi notifiés.

Tout le monde est content : vous, puisque l'on vous laisse plus tranquille, et vos fans parce qu'ils savent qu'ils ne rateront pas la chanson.

C'est une analogie réelle à un problème courant de programmation :

1. Un "producteur de code" qui réalise quelque chose mais nécessite du temps. Par exemple, un code qui charge des données à travers un réseau. C'est le "chanteur".
2. Un "consommateur de code" qui attend un résultat du "producteur de code" quand il est prêt. Beaucoup de fonctions peuvent avoir besoin de ce résultat. Ces fonctions sont les "fans".
3. Une *promesse* (promise) est un objet spécial en javascript qui lie le "producteur de code" et le "consommateur de code" ensemble. En comparant à notre analogie c'est la "liste d'abonnement". Le "producteur de code" prends le temps nécessaires pour produire le résultat promis, et la "promesse" donne le résultat disponible pour le code abonné quand c'est prêt.

L'analogie n'est pas la plus correct, car les promesses en Javascript sont un peu plus complexes qu'une simple liste d'abonnement : elles ont d'autres possibilités mais aussi des certaines limitations. Toutefois c'est suffisant pour débuter.

La syntaxe du constructeur pour une promesse est :

```js
let promise = new Promise(function(resolve, reject) {
  // L'exécuteur (le code produit, le "chanteur")
});
```

La fonction passée à `new Promise` est appelée l'*exécuteur*. Quand `new Promise` est créée, elle est lancée automatiquement. Elle contient le producteur de code, qui doit produire un résulat final. Dans l'analogie au-dessus : l'éxécuteur est le "chanteur".

Ses arguments `resolve` (tenir) et `reject` (rompre) sont les fonctions de retours directement fournies par Javascript. Notre code est inclus seulement dans l'exécuteur.

Quand l'exécuteur obtient un résultat, qu'il soit rapide ou pas, cela n'a pas d'importance, il appellera une des deux fonctions callbacks :

- `resolve(value)` -  si la tâche s'est terminée avec succés, avec le résultat `value`.
- `reject(error)` - si une erreur est survenue, `error` est l'objet erreur.

Donc, pour résumer : l'exécuteur s'exécute automatiquement et tente d’effectuer un travail. Ensuite, il devrait appeler `resolve` s'il a réussi ou `reject` s'il y avait une erreur.

L'objet `promise` retourné par le constructeur `new Promise` a des propriétés internes :

<<<<<<< HEAD
- `state` (état) - initiallement à `"pending"` (en attente), se change soit en `"fulfilled"` (tenue) lorsque `resolve` est appelé ou `"rejected"` (rompue) si `reject` est appelé.
- `result` - initialement à `undefined` se change à `value` quand `resolve(value)` est appelé ou `error` quand `reject(error)` est appelé.
=======
- `state` — initially `"pending"`, then changes to either `"fulfilled"` when `resolve` is called or `"rejected"` when `reject` is called.
- `result` — initially `undefined`, then changes to `value` when `resolve(value)` is called or `error` when `reject(error)` is called.
>>>>>>> 18b1314af4e0ead5a2b10bb4bacd24cecbb3f18e

Ainsi l'éxécuteur changera la promesse à un de ces états :

![](promise-resolve-reject.svg)

Plus tard nous verrons comment les "fans" peuvent s'abonner à ces changements.

Voici un exemple d'un constructeur d'une promesse et d'une fonction exécutrice simple avec un "code produit" qui prends du temps (utilisant `setTimeout`) :

```js run
let promise = new Promise(function(resolve, reject) {
  // la fonction est exécutée automatiquement quand la promesse est construite

  // On signale au bout d'une seconde que la tâche est terminée avec le résultat "done"
  setTimeout(() => *!*resolve("done")*/!*, 1000);
});
```
On peut voir deux chose en lançant le code ci-dessus :

1. L'exécuteur est appelé automatiquement et immédiatement (avec `new Promise`).
2. L'exécuteur reçoit deux arguments : `resolve` et `reject` - ces deux fonctions sont pré-définies par le moteur Javascript, ainsi nous n'avons pas besoin de les créer. Nous devons seulement appelé l'une ou l'autre quand le résultat est prêt.

<<<<<<< HEAD
    Après une seconde de "traitement" l'exécuteur appelle `resolve("done")` pour produire le résultat. Cela change l'état de l'objet `promise` :
=======
1. The executor is called automatically and immediately (by `new Promise`).
2. The executor receives two arguments: `resolve` and `reject`. These functions are pre-defined by the JavaScript engine, so we don't need to create them. We should only call one of them when ready.

    After one second of "processing", the executor calls `resolve("done")` to produce the result. This changes the state of the `promise` object:
>>>>>>> 18b1314af4e0ead5a2b10bb4bacd24cecbb3f18e

    ![](promise-resolve-1.svg)

Nous avons vu un exemple d'une tâche terminée avec succés, une promesse "tenue".

Voyons maintenant un exemple d'un exécuteur rompant la promesse avec une erreur :

```js
let promise = new Promise(function(resolve, reject) {
  // On signale après 1 seconde que la tâche est terminée avec une erreur
  setTimeout(() => *!*reject(new Error("Whoops!"))*/!*, 1000);
});
```

L'appel a `reject(...)` change l'object promesse à l'état `"rejected"` :

![](promise-reject-1.svg)

Pour résumer, l'exécuteur devrait réaliser une tâche (normalement quelque chose qui prends du temps) puis appelle `resolve` ou `reject` pour changer l'état de l'objet promesse correspondant.

Une promesse qui est soit tenue ou rejetée est appelée "settled" (acquitttée) opposé à une promesse initialisée à "en attente".

````smart header="Il ne peut y avoir qu'un seul résultat ou une erreur"
L'exécuteur devrait appeler seulement une fois `resolve` ou `reject`. N'importe quel changement d'état est définitif.

Les appels supplémentaires à `resolve` et `reject` sont ignorés :

```js
let promise = new Promise(function(resolve, reject) {
*!*
  resolve("done");
*/!*

  reject(new Error("…")); // ignoré
  setTimeout(() => resolve("…")); // ignoré
});
```

L'idée est que la tâche exécutée par un exécuteur ne peut avoir qu'un seul résultat ou une erreur.

De plus, `resolve`/`reject` attendent qu'un seul argument (ou aucun) et ignorera les arguments suivants.
````

```smart header="Rompre avec l'objet `Error`"
Dans le cas ou quelque chose se passe mal, l'exécuteur doit appeler `reject`. Cela est possible avec n'importe type d'argument (comme pour `resolve`). Mais  il est plutôt recommandé d'utiliser l'objet `Error` (ou les object en héritant). La raison va vous paraître évident dans un instant.
```

````smart header="Appel de `resolve`/`reject` immédiat"
En pratique, un exécuteur réalise normalement une opération asynchrone et appelle `resolve`/`reject` après un certain temps, mais il n'est pas obligatoire d'être asynchrone. On peut aussi appeler immédiatement `resolve` ou `reject`, comme cela :

```js
let promise = new Promise(function(resolve, reject) {
  // La tâche ne prends pas de temps
  resolve(123); // rend immédiatement le résultat : 123
});
```

Par exemple, cela peut arriver quand nous commençons une tâche mais nous voyons que la tâche est déja réalisée et en cache.

Pas de soucis. Nous acquittons immédiatement la promesse.
````

```smart header="Le `state` et `result` est interne"
Les propriétés `state` et `result` de l'objet `Promise` sont internes. Nous ne pouvons directement accéder à celles-ci. Nous pouvons utiliser `.then`/`.catch`/`.finally` pour cela. Elles sont décrites ci-dessous.
```

<<<<<<< HEAD
## Les consommateurs: then, catch, finally

Un objet promesse permet le lien entre l'exécuteur (le "code produit" ou "chanteur") et les fonctions consommatrices (les "fans"), lesquels recevront un résultat ou une erreur. Ces fonctions peuvent s'abonner (subscribed) utilisant les méthodes `.then`, `.catch` and `.finally`.
=======
## Consumers: then, catch

A Promise object serves as a link between the executor (the "producing code" or "singer") and the consuming functions (the "fans"), which will receive the result or error. Consuming functions can be registered (subscribed) using the methods `.then` and `.catch`.
>>>>>>> 18b1314af4e0ead5a2b10bb4bacd24cecbb3f18e

### then (alors)

Le plus important, le plus crucial est `.then`.

La syntaxe est :

```js
promise.then(
  function(result) { *!*/* gère un résultat correct */*/!* },
  function(error) { *!*/* gère une erreur */*/!* }
);
```

<<<<<<< HEAD
Le premier argument de `.then` est une fonction qui se lance si la promesse est tenue, et reçoit le résultat.

Le deuxième argument de `.then` est une fonction qui se lance si la promesse est rompue, et reçoit l'erreur.
=======
The first argument of `.then` is a function that runs when the promise is resolved and receives the result.

The second argument of `.then` is a function that runs when the promise is rejected and receives the error.
>>>>>>> 18b1314af4e0ead5a2b10bb4bacd24cecbb3f18e

Par exemple, voyons la réponse à un une requête correctement tenue :

```js run
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => resolve("done!"), 1000);
});

// resolve lance la première fonction dans .then
promise.then(
*!*
  result => alert(result), // affiche "done!" après 1 seconde
*/!*
  error => alert(error) // ne se lance pas
);
```

La première fonction s'est exécutée.

Et dans le cas d'un rejet -- la deuxième seulement s'exécute :

```js run
let promise = new Promise(function(resolve, reject) {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

// reject lance la seconde fonction dans .then
promise.then(
  result => alert(result), // ne se lance pas
*!*
  error => alert(error) // affiche "Error: Whoops!" après 1 seconde
*/!*
);
```
Si nous sommes seulement intéressés aux promesses tenues, nous pouvons alors seulement fournir une fonction en argument à `.then` :

```js run
let promise = new Promise(resolve => {
  setTimeout(() => resolve("done!"), 1000);
});

*!*
promise.then(alert); // affiche "done!" après 1 seconde
*/!*
```

### catch

Si nous sommes seulement intéressés aux erreurs, alors nous pouvons mettre `null` comme premier argument : `.then(null, fonctionGerantLErreur)`. Ou nous pouvons utiliser `.catch(fonctionGerantLErreur)`, qui revient au même :

```js run
let promise = new Promise((resolve, reject) => {
  setTimeout(() => reject(new Error("Whoops!")), 1000);
});

*!*
// .catch(f) est similaire à promise.then(null, f)
promise.catch(alert); // affiche "Error: Whoops!" après 1 seconde
*/!*
```

L'appel à `.catch(f)` est complétement analogue à `.then(null, f)`, c'est juste un raccourci.

## Cleanup: finally

Comme il y a un terme `finally` dans un `try {...} catch {...}`, il y a des `finally` dans les promesses.

<<<<<<< HEAD
L'appel à  `.finally(f)` est similaire à `.then(f, f)` dans le sens où `f` se lance toujours quand la promesse est aquittée : qu'elle soit tenue ou rompue.

`finally` est un bon moyen pour nettoyer, i.e arrêter un indicateur de chargement, car ils ne sont plus utiles, qu'importe le résultat.

Comme cela :

```js
new Promise((resolve, reject) => {
  /* faire quelque chose qui prend du temps puis lancer resolve/reject */
=======
The call `.finally(f)` is similar to `.then(f, f)` in the sense that `f` runs always, when the promise is settled: be it resolve or reject.

The idea of `finally` is to set up a handler for performing cleanup/finalizing after the previous operations are complete.

E.g. stopping loading indicators, closing no longer needed connections, etc.

Think of it as a party finisher. No matter was a party good or bad, how many friends were in it, we still need (or at least should) do a cleanup after it.

The code may look like this:

```js
new Promise((resolve, reject) => {
  /* do something that takes time, and then call resolve or maybe reject */
>>>>>>> 18b1314af4e0ead5a2b10bb4bacd24cecbb3f18e
})
*!*
  // se lance quand la promesse est acquittée, peu importe si celle-ci est tenue ou rompue
  .finally(() => stop loading indicator)
  // so the loading indicator is always stopped before we go on
*/!*
  .then(result => show result, err => show error)
```

<<<<<<< HEAD
Cela dit, `finally(f)` n'est pas exactement un alias de `then(f,f)`. Il existe quelques différences subtiles :

1. Un gestionnaire `finally` ne prends pas d'arguments. Dans un `finally` nous ne savons pas si la promesse est tenue ou rompue. Cela ne pose pas de soucis, notre tâche est habituellement de réaliser les procédures finales "générales".
2. Un gestionnaire `finally` passe le résultat ou l'erreur au gestionnaire suivant.

    Par exemple, le résultat passant à travers `finally` vers `then` :
=======
Please note that `finally(f)` isn't exactly an alias of `then(f,f)` though.

There are important differences:

1. A `finally` handler has no arguments. In `finally` we don't know whether the promise is successful or not. That's all right, as our task is usually to perform "general" finalizing procedures.

    Please take a look at the example above: as you can see, the `finally` handler has no arguments, and the promise outcome is handled by the next handler.
2. A `finally` handler "passes through" the result or error to the next suitable handler.

    For instance, here the result is passed through `finally` to `then`:

>>>>>>> 18b1314af4e0ead5a2b10bb4bacd24cecbb3f18e
    ```js run
    new Promise((resolve, reject) => {
      setTimeout(() => resolve("value"), 2000);
    })
<<<<<<< HEAD
      .finally(() => alert("Promise ready"))
      .then(result => alert(result)); // <-- .then gère le résultat
    ```
    Et là avec une erreur dans la promesse, passsant à travers `finally` vers `catch` :
=======
      .finally(() => alert("Promise ready")) // triggers first
      .then(result => alert(result)); // <-- .then shows "value"
    ```

    As you can see, the `value` returned by the first promise is passed through `finally` to the next `then`.

    That's very convenient, because `finally` is not meant to process a promise result. As said, it's a place to do generic cleanup, no matter what the outcome was.

    And here's an example of an error, for us to see how it's passed through `finally` to `catch`:
>>>>>>> 18b1314af4e0ead5a2b10bb4bacd24cecbb3f18e

    ```js run
    new Promise((resolve, reject) => {
      throw new Error("error");
    })
<<<<<<< HEAD
      .finally(() => alert("Promise ready"))
      .catch(err => alert(err));  // <-- .catch gère l'objet error
    ```

    Cela est vraiment pratique, en effet `finally` n'est pas censer gérer le résultat d'une promesse. Donc il passe à travers.

    Nous parlerons plus en détails de l'enchaînements de promesses et le passage de résultats à travers les gestionnaires dans le prochain chapitre.

````smart header="Nous pouvons attacher des gestionnaires à des promesses configurées"
Si une promesse est en attente, les gestionnaires `.then/catch/finally` l'attendent. Sinon, si une promesse est déjà configurée, ils exécutent simplement :
=======
      .finally(() => alert("Promise ready")) // triggers first
      .catch(err => alert(err));  // <-- .catch shows the error
    ```

3. A `finally` handler also shouldn't return anything. If it does, the returned value is silently ignored.

    The only exception to this rule is when a `finally` handler throws an error. Then this error goes to the next handler, instead of any previous outcome.

To summarize:

- A `finally` handler doesn't get the outcome of the previous handler (it has no arguments). This outcome is passed through instead, to the next suitable handler.
- If a `finally` handler returns something, it's ignored.
- When `finally` throws an error, then the execution goes to the nearest error handler.

These features are helpful and make things work just the right way if we use `finally` how it's supposed to be used: for generic cleanup procedures.

````smart header="We can attach handlers to settled promises"
If a promise is pending, `.then/catch/finally` handlers wait for its outcome.

Sometimes, it might be that a promise is already settled when we add a handler to it.

In such case, these handlers just run immediately:
>>>>>>> 18b1314af4e0ead5a2b10bb4bacd24cecbb3f18e

```js run
// la prommesse est acquittée immédiatement à la création
let promise = new Promise(resolve => resolve("done!"));

promise.then(alert); // done! (s'affiche immédiatement)
```

Notez que cela rend les promesses plus puissantes que le scénario réel de "liste d'abonnement". Si le chanteur a déjà sorti sa chanson et qu'une personne s'inscrit sur la liste d'abonnement, elle ne recevra probablement pas cette chanson. Les abonnements dans la vraie vie doivent être effectués avant l'événement.

Les promesses sont plus flexibles. Nous pouvons ajouter des gestionnaires à tout moment : si le résultat est déjà là, ils s'exécutent simplement.
````

<<<<<<< HEAD
Ensuite, voyons des exemples plus pratiques pour lesquels les promesses nous aident à écrire du code asynchrone.

## Example: loadScript [#loadscript]

Nous avons la fonction `loadScript` pour charger un script d'un chapitre précédent.
=======
## Example: loadScript [#loadscript]

Next, let's see more practical examples of how promises can help us write asynchronous code.

We've got the `loadScript` function for loading a script from the previous chapter.
>>>>>>> 18b1314af4e0ead5a2b10bb4bacd24cecbb3f18e

Pour rappel voyons la solution avec des fonctions de retour :

```js
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Script load error for ${src}`));

  document.head.append(script);
}
```

Re-écrivons la avec une promesse.

La nouvelle fonction `loadScript` ne nécessite aucune fonction de retour. À la place, elle va créer et retournera une promesse qui s'acquittera lorque le chargement sera complet. Le code externe peut ajouter des gestionnaires (fonction s'abonnant) à celle-ci en utilisant `.then`.

```js run
function loadScript(src) {
  return new Promise(function(resolve, reject) {
    let script = document.createElement('script');
    script.src = src;

    script.onload = () => resolve(script);
    script.onerror = () => reject(new Error(`Script load error for ${src}`));

    document.head.append(script);
  });
}
```

Utilisation:

```js run
let promise = loadScript("https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js");

promise.then(
  script => alert(`${script.src} is loaded!`),
  error => alert(`Error: ${error.message}`)
);

promise.then(script => alert('Another handler...'));
```

On peut remarquer immédiatement quelques avantages par rapport aux fonctions de retour :

| Promesses | Fonctions de retour |
|-----------|----------------------|
| Les promesses nous permettent de faire des choses dans un ordre naturel. D'abord, nous lançons `loadScript(script)`, puis avec `.then` nous codons quoi faire avec le résultat. | Nous devons avoir une fonction de retour à notre disposition quand nous appelons `loadScript(script, callback)`. En d'autres mots, nous devons savoir quoi faire du résultat *avant* que `loadScript` est appelé. |
| Nous pouvons appeler `.then` sur une promesse autant de temps fois que nécessaires. À chaque fois, nous ajoutons un nouveau "fan", une nouvelle fonction s'abonnant à la "liste d'abonnés". Nous en verrons plus à ce sujet dans le prochain chapitre : [](info:promise-chaining). | Il ne peut y avoir qu'une seule fonction de retour. |

Les promesses nous permettent donc d'avoir plus de sens et une meilleure flexibilité. Mais il y a plus. Nous allons voir cela dans les chapitres suivants.
