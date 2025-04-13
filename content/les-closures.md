# Les Clôtures (Closures)

<aside>
## Définition
Une **clôture** (*closure*) est une fonction qui **retient l'accès** aux variables de son **environnement lexical** même après que la fonction parente ait terminé son exécution. Cela permet à la fonction de "se souvenir" de l'état des variables du contexte dans lequel elle a été créée, comme une petite mémoire interne à une fonction.

[Closures dans les framework front](/content/les-closures/Closures-dans-les-framework-front.md)
</aside>

### Concept clé

Les clôtures permettent aux fonctions internes de "capturer" l'environnement de la fonction parente. Ce mécanisme préserve les variables locales de cette dernière, même si elle est terminée, rendant ces variables accessibles depuis la fonction interne.

- Une fonction peut accéder aux variables déclarées dans sa portée (scope) ainsi qu'à celles de ses fonctions parentes.
- Une clôture est souvent créée lorsqu'une fonction interne est renvoyée par une fonction externe.

```jsx
function createCounter() {
    let count = 0;
    return function() {
        count++;
        return count;
    };
}

const counter = createCounter();
console.log(counter());
console.log(counter());
```

## Exemple

```jsx
function outerFunction(x) {
  let y = 10;
  function innerFunction() {
    console.log(x + y);
  }
  return innerFunction;
}

const closure = outerFunction(5);
closure(); // Affiche 15
```

Dans cet exemple, la fonction `innerFunction` est une clôture. Elle retient l'accès aux variables `x` et `y`, même après la fin de l'exécution de `outerFunction`.

### Fonctionnement interne et implications

- Chaque fonction en JavaScript crée un **environnement lexical** qui contient ses variables locales.
- Les fonctions internes retiennent l'accès aux variables de leur fonction parente grâce à une **référence** vers cet environnement lexical.
- Cela permet de **préserver l'état** des variables même après que la fonction parente ait terminé son exécution.

## Applications courantes

Les clôtures sont très utiles dans plusieurs situations :

- **Encapsulation et données privées** : Permet de restreindre l'accès direct à certaines données et de contrôler l'accès à ces données via des méthodes spécifiques.
- **Création de fonctions fabriques** : Facilite la génération de nouvelles fonctions personnalisées en capturant un état initial.
- **Gestion d'état** : Pratique pour gérer un état persistant dans des fonctions comme un compteur.
- **Callbacks et gestion d'événements** : Les clôtures sont largement utilisées dans les callbacks et la gestion d'événements pour maintenir le contexte.

## Exemples : Compteur privé

Voici un exemple classique de qui montre la magie des **clôtures 🪄**, utilisé pour **encapsuler** un compteur privé auquel on peut accéder uniquement via des **méthodes** spéciales.

```jsx
function createCounter() {
  let count = 0; // Variable privée
  return {
    increment: function() {
      count++;
    },
    decrement: function() {
      count--;
    },
    getCount: function() {
      return count;
    }
  };
}

const counter = createCounter();
counter.increment();
counter.increment();
console.log(counter.getCount()); // Affiche 2
console.log(count); // ReferenceError: count is not defined
```

Ici, `createCounter` retourne un objet contenant trois méthodes. Ces méthodes peuvent manipuler la variable `count`, qui reste **inaccessible** depuis l'extérieur (elle est privée à l'intérieur de la fonction `createCounter`).

## Application et avantages des clôtures

- **Encapsulation** : Les clôtures permettent de restreindre l'accès à certaines données, tout en les rendant accessibles via des fonctions spécifiques.
- **Gestion de l'état** : Elles permettent de conserver un état entre plusieurs appels de fonction sans avoir besoin de variables globales.
- **API modulaires** : En cachant des données internes, les clôtures facilitent la création de modules JavaScript autonomes et sécurisés.

## Attentions !

- **Fuites de mémoire** : Notamment dans le cas ou des des fonctions “clôturées” référencent des objets lourds, cela peut entraîner des **fuites de mémoire**, car ces objets ne seront pas libérés tant que la clôture est active.
- **Lisibilité du code** : Un abus des clôtures peut rendre le code difficile  lire et à déboguer, surtout lorsqu'elles sont imbriquées ou utilisées dans des boucles.

### Exemple de fuite de mémoire

```jsx
function createHeavyClosure() {
  const largeArray = new Array(1000000).fill('memory leak');
  return function() {
    console.log('Using closure with heavy data');
  };
}

const heavyClosure = createHeavyClosure();
// Si "heavyClosure" n'est jamais supprimée, "largeArray" reste en mémoire
```

## Bonnes pratiques

- Utilisez les clôtures pour **encapsuler l'état** ou des données privées, mais avec parcimonie.
- **Évitez de créer des clôtures dans des boucles**, car cela peut entraîner des comportements inattendus si la variable de boucle est partagée entre toutes les clôtures.
- Nommez clairement les fonctions internes pour améliorer la **lisibilité** et faciliter le **débogage**.

<aside>


## **Conclusion**

Les clôtures sont un concept fondamental en JavaScript qui permet de créer des fonctions avec **accès persistant aux variables de leur environnement**. Elles sont essentielles pour de nombreux cas d'utilisation comme l'encapsulation, la gestion d'état, ou les callbacks. Cependant, il est important de les utiliser de manière **raisonnée** pour éviter les problèmes de lisibilité et de performances.

</aside>

## Exercices

### Exercice 1 : Validation de mot de passe

1. **Créez une fonction `createPasswordValidator`** qui prend en paramètre une **expression régulière (REGEX)** pour valider un mot de passe. Elle doit retourner une fonction qui accepte un mot de passe en argument et valide celui-ci selon l'expression régulière fournie.

**Utilisation finale:**

```jsx
const passwordValidator = createPasswordValidator(/^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$/);
console.log(passwordValidator("Password123")); // true
console.log(passwordValidator("password")); // false
```

### Exercice 2 : Gestion de comptes bancaires

1. **Implémentez une fonction `createBank`** qui gère des comptes bancaires avec des fonctionnalités de **dépôt**, **retrait**, et **consultation du solde**. Le solde doit être privé et modifiable uniquement via les fonctions retournées par `createBank`.

**Utilisation finale:**

```jsx
const bankAccount = createBank();
bankAccount.deposit(100);
bankAccount.withdraw(50);
console.log(bankAccount.getBalance()); // 50
console.log(bankAccount.balance); // ReferenceError: balance is not defined
```

### Exercice 3 : Système de gestion de tâches

1. **Développez un système de gestion de tâches** en utilisant des clôtures. Créez une fonction `createTaskManager` qui permet d'**ajouter des tâches**, de les **marquer comme complétées**, et de **lister les tâches** en cours et complétées. Utilisez des clôtures pour **garder la liste des tâches privée**.

```jsx
const taskManager = createTaskManager();
taskManager.addTask("Learn JavaScript");
taskManager.addTask("Create a project");
taskManager.completeTask("Learn JavaScript");
console.log(taskManager.getTasks()); // { pending: ["Create a project"], completed: ["Learn JavaScript"] }
```

### Objectifs des exercices :

- **Exercice 1** : Apprendre à utiliser les clôtures pour encapsuler une logique de validation dans une fonction personnalisée.
- **Exercice 2** : Utiliser les clôtures pour créer un système sécurisé de gestion de comptes bancaires avec des méthodes pour manipuler un solde privé.
- **Exercice 3** : Mettre en œuvre un gestionnaire de tâches en utilisant des clôtures pour garder la liste des tâches privée et gérer l'état des tâches.

### Tips en cas de besoin 😉

- **Exercice 1 (Validation de mot de passe)** :
    - Utilise l'opérateur `new RegExp()` pour créer une regex dynamique si besoin.
    - N'oublie pas que `test()` est une méthode des regex qui permet de vérifier si un mot de passe correspond à l'expression régulière.
- **Exercice 2 (Gestion de comptes bancaires)** :
    - La variable privée `balance` doit être accessible uniquement via les méthodes de dépôt, retrait, et consultation du solde.
    - Penses à ajouter une validation pour vérifier si le solde est suffisant avant de permettre un retrait.
- **Exercice 3 (Gestion des tâches)** :
    - Utilise un tableau privé dans la clôture pour stocker les tâches.
    - Sépare les tâches complétées des tâches en cours en deux listes internes, et fournis des méthodes pour les récupérer.

---