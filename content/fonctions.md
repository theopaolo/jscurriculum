# Fonctions avancé

## First-Class Functions

En JavaScript, les fonctions sont des "**citoyens de première classe**" ("first-class citizens"), ce qui signifie qu'elles peuvent être traitées comme n'importe quelle autre valeur. Cela inclut la capacité à être **assignées à des variables**, **passées en argument** ou **retournées depuis d'autres fonctions**.

**Exemple**

```jsx
// Assigner une fonction à une variable
const greet = function(name) {
  return `Hello, ${name}!`;
};

// Passer une fonction comme argument
function executeFunction(fn, arg) {
  return fn(arg);
}

console.log(executeFunction(greet, "Alice")); // "Hello, Alice!"

// Retourner une fonction depuis une autre fonction
function createMultiplier(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = createMultiplier(2);
console.log(double(5)); // 10
```

### Fonctionnement interne et implications

- En JavaScript, les **fonctions** sont des objets de type `Function`.
- Elles peuvent avoir des **propriétés** et des **méthodes**, ce qui en fait des valeurs manipulables comme les autres.
- Cela permet la **programmation fonctionnelle**, facilitant l'utilisation de **fonctions d'ordre supérieur** et des **closures**.

## Higher-Order Functions

Les **fonctions d'ordre supérieur** sont des fonctions qui peuvent prendre d'autres fonctions en **arguments** ou en **retourner**.

**Exemple**

```jsx
// Fonction qui prend une fonction comme argument
function applyOperation(x, y, operation) {
  return operation(x, y);
}

const add = (a, b) => a + b;
const multiply = (a, b) => a * b;

console.log(applyOperation(5, 3, add)); // 8
console.log(applyOperation(5, 3, multiply)); // 15

// Fonction qui retourne une fonction
function createGreeter(greeting) {
  return function(name) {
    return `${greeting}, ${name}!`;
  };
}

const casualGreeter = createGreeter("Hi");
const formalGreeter = createGreeter("Good day");

console.log(casualGreeter("Bob")); // "Hi, Bob!"
console.log(formalGreeter("Sir")); // "Good day, Sir!"
```

### Application et avantages

- Permettent une grande **réutilisabilité** et **modularité** du code.
- Facilitent des patterns comme la **composition de fonctions**.
- Très utilisés dans des méthodes comme **map**, **filter**, et **reduce** des tableaux, pour appliquer des transformations élégantes et concises.

## Callbacks

Les **callbacks** sont des fonctions passées en arguments à d'autres fonctions, et qui seront **exécutées ultérieurement**.

### Exemples

```jsx
// Exemple simple de callback
function fetchData(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "John Doe" };
    callback(data);
  }, 1000);
}

fetchData((result) => {
  console.log(result); // { id: 1, name: "John Doe" }
});

// Gestion d'erreurs avec les callbacks
function fetchDataWithError(callback) {
  setTimeout(() => {
    const success = Math.random() > 0.5;
    if (success) {
      callback(null, { id: 1, name: "John Doe" });
    } else {
      callback(new Error("Failed to fetch data"));
    }
  }, 1000);
}

fetchDataWithError((error, data) => {
  if (error) {
    console.error("Error:", error.message);
  } else {
    console.log("Data:", data);
  }
});
```

### Fonctionnement et considérations

- Les **callbacks** sont la **base de la programmation asynchrone** en JavaScript.
- Ils permettent d'**exécuter du code** une fois qu'une **opération asynchrone** est terminée, par exemple après une requête réseau.
- Le **"callback hell"** est un problème qui peut survenir lorsque les callbacks sont **imbriqués**, rendant le code difficile à lire et à maintenir.
- Les **Promises** et **async/await** ont été introduits pour améliorer la lisibilité et simplifier la gestion des erreurs par rapport aux callbacks.

## Exercices pratiques

### Exercice 1 : Création de la fonction d'ordre supérieur `createLogger`

1. **Création de `createLogger`** : Cette fonction d'ordre supérieur prend un **préfixe** en argument et retourne une fonction de **logging** qui affiche un message avec le préfixe.

### Exercice 2 : Extension de `createLogger` avec un callback optionnel

1. **Ajouter un callback optionnel à `createLogger`** :
    - Étend la fonction `createLogger` pour prendre un **callback** optionnel.
    - Le callback est appelé avec le message complet avant l'affichage.
    - Si le callback retourne `false`, le message **ne doit pas** être affiché, permettant un **filtrage personnalisé**.

### Exercice 3 : Implémentation de la fonction `sequential`

1. **Implémenter une fonction `sequential`** :
    - La fonction `sequential` prend un **tableau de fonctions asynchrones** et les exécute **séquentiellement**.
    - Chaque fonction doit recevoir un **callback** qui doit être appelé une fois l'opération terminée.
    - Utilise une approche récursive pour **enchaîner** les fonctions, en gérant les erreurs tout en continuant l'exécution des autres tâches.

---

### Objectifs des exercices :

- **Exercice 1** : Utiliser des **fonctions d'ordre supérieur** pour créer des outils réutilisables (loggers).
- **Exercice 2** : Introduire le concept de **filtrage dynamique** des messages de log via un **callback optionnel**.
- **Exercice 3** : Maîtriser la **programmation asynchrone** avec des callbacks, en créant une solution pour **exécuter des fonctions asynchrones séquentiellement**.

*Ces exercices t'aident à comprendre des concepts importants en JavaScript tels que les **fonctions de première classe**, les **fonctions d'ordre supérieur**, et la **programmation asynchrone***

---

### Tips en cas de besoin 😉

- **Logger de Première Classe (`createLogger`)** :
    - La première fonction `createLogger` crée un **logger simple** qui ajoute un **préfixe** à chaque message.
    - Cela permet de créer des logs spécifiques (`INFO`, `ERROR`, etc.) pour différentes parties de l'application.
- **Logger Étendu avec Filtrage** :
    - La version étendue ajoute un **callback optionnel** qui est appelé avant l'affichage.
    - Si le callback retourne `false`, le message **n'est pas affiché**.
    - Cela permet de **filtrer les logs** selon des critères personnalisés (par exemple, afficher uniquement les messages courts).
- **Exécution Séquentielle des Fonctions Asynchrones (`sequential`)** :
    - La fonction `sequential` utilise une approche **récursive** pour exécuter chaque fonction dans le tableau.
    - **next()** appelle la fonction suivante une fois que la précédente est terminée, **enchaînant** ainsi les opérations asynchrones.
    - En cas d'**erreur**, celle-ci est loguée, mais les opérations suivantes continuent.
    - Cela assure une exécution séquentielle, même en cas d'échec partiel.