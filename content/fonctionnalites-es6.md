# Fonctionnalités ES6 et plus

<aside>
<img src="https://www.notion.so/icons/info-alternate_green.svg" alt="https://www.notion.so/icons/info-alternate_green.svg" width="40px" />

ES6 ou ECMAScript 2015 est la seconde révision la plus importante de JavaScript, elle introduit multiples fonctionnalités nouvelles au langage le rendant plus moderne, puissant et facile à utiliser.

</aside>

Depuis **2016**, les versions d'ECMAScript sont nommées par année (ex. ECMAScript 2016, 2017, etc.). Depuis cette année, les évolutions du langage sont régulières et se concentrent sur des ajouts progressifs et des petites améliorations, plutôt que sur des révolutions majeures.

![JavaScript ES6 features.png](Fonctionnalite%CC%81s%20ES6%20et%20plus%201253324cc3d7805fa8bef934d994b274/JavaScript_ES6_features.png)

## Les nouveautés révolutionnaires de ES6 (ECMAScript 2015)

- `let` et `const` : Pour la déclaration de variables avec portée de bloc.
- Function fléché `=>` : Syntaxe plus concise et `this` lexical.
- **L'opérateur de propagation (`...`)** : Pour les objets et les tableaux (spread/rest).
- **Destructuration** : Extraction de valeurs à partir d'objets et de tableaux.
- **Boucle `for...of`** : Itération sur des objets itérables.
- **`Map` et `Set`** : Structures de données avancées.
- **Valeurs par défaut des paramètres** : Permet de définir des valeurs par défaut pour les arguments de fonction.
- **`Array.entries()`, `Array.from()`** : Nouvelles méthodes utiles pour travailler avec des tableaux.
- **Modules (`import` et `export`)** : Modularisation du code.

**ECMAScript 2017** a introduit :

- **`Object.entries()` et `Object.values()`** : Pour travailler facilement avec les objets.
- **L'objet `Promise`** : Pour la gestion des opérations asynchrones.

<aside>
<img src="https://www.notion.so/icons/info-alternate_green.svg" alt="https://www.notion.so/icons/info-alternate_green.svg" width="40px" />

Les versions récentes de ECMAScript (à partir de 2018) n'apportent pas de révolutions majeures mais continuent d'améliorer le langage avec des fonctionnalités utiles. Les versions de 2019 à 2024 sont encore en cours d'implémentation dans certains navigateurs, et il est souvent nécessaire d'utiliser des polyfills pour garantir leur compatibilité en production.

</aside>

## Arrow Functions

Les **arrow functions** offrent une syntaxe raccourcie pour écrire des fonctions en JavaScript, tout en modifiant le comportement du mot-clé `this`.

```jsx
// Fonction traditionnelle
function add(a, b) {
  return a + b;
}

// Arrow function équivalente
const add = (a, b) => a + b;

// Arrow function avec bloc de code
const greet = (name) => {
  console.log(`Hello, ${name}!`);
  return `Greetings to ${name}`;
};

// Arrow function et this
const obj = {
  name: "Alice",
  sayHello: function () {
    setTimeout(() => {
      console.log(`Hello, ${this.name}`);
    }, 1000);
  },
};
```

Dans l'exemple ci-dessus, l'utilisation de `setTimeout()` avec une **arrow function** permet de conserver la valeur de `this` provenant de `sayHello`. Cela simplifie l'accès aux propriétés de l'objet sans devoir lier `this` manuellement.

- Les **arrow functions** **n'ont pas** de `this` propre. Elles **héritent** le `this` du contexte englobant, ce qui est souvent appelé **lexical `this`**.

### **Fonctionnement interne et particularités**

- Elles sont particulièrement **utiles** pour les **callbacks** et les méthodes qui nécessitent de préserver le contexte parent, comme dans l'exemple de `setTimeout()` ci-dessus.
- Contrairement aux fonctions traditionnelles, les arrow functions **n'ont pas** accès aux objets `arguments`, `super`, ou `new.target`.
- Elles **ne peuvent pas** être utilisées comme **constructeurs**, donc il n'est pas possible de les appeler avec le mot-clé `new`.
- Elles **ne peuvent pas changer** la valeur de `this` même lorsqu'on utilise les méthodes `call()`, `apply()`, ou `bind()`.

### Cas d'utilisation des Arrow Functions

- **Callbacks** : Les arrow functions sont parfaites pour les callbacks, car elles permettent de conserver le contexte d'exécution plus simplement.
    
    ```jsx
    const numbers = [1, 2, 3];
    const squaredNumbers = numbers.map(num => num * num); // [1, 4, 9]
    ```
    
- **Méthodes asynchrones** : Les arrow functions sont également utiles pour les opérations asynchrones nécessitant de conserver le `this`.
    
    ```jsx
    class Timer {
      constructor() {
        this.seconds = 0;
      }
    
      start() {
        setInterval(() => {
          this.seconds++;
          console.log(this.seconds);
        }, 1000);
      }
    }
    
    const myTimer = new Timer();
    myTimer.start(); // Affiche 1, 2, 3, etc., chaque seconde
    ```
    
    Ici, `this` fait référence à l'instance de `Timer` car l'arrow function hérite du contexte lexical, contrairement à une fonction traditionnelle qui aurait besoin de `bind()` pour le faire.
    
- **Éviter les problèmes de `this`** : Les arrow functions sont souvent utilisées dans les classes et les objets pour éviter la confusion autour du mot-clé `this`.

### Quand **ne pas** utiliser les Arrow Functions

- **Méthodes d'objet** : Si une fonction est destinée à être une méthode d'objet et que vous avez besoin d'un `this` qui soit l'instance actuelle de l'objet, une **fonction traditionnelle** est plus appropriée.
    
    ```jsx
    const car = {
      brand: 'Toyota',
      getBrand() {
        return this.brand;
      }
    };
    ```
    
- **Constructeurs** : Les arrow functions ne peuvent pas être utilisées comme constructeurs, il faut donc privilégier des fonctions traditionnelles pour créer des objets via `new`.

## Destructuring

Le **destructuring** permet d'extraire des valeurs d'objets ou de tableaux et de les assigner à des variables individuelles, facilitant ainsi la manipulation des données.

```jsx
// Destructuring d'objet
const person = { name: 'Alice', age: 30, address: { city: 'Paris', country: 'France' } };
const { name, age, address: { city } } = person; // Extraction de 'name', 'age' et 'city'

// Destructuring avec renommage
const { name: fullName } = person; // 'fullName' contient la valeur de 'person.name'

// Destructuring de tableau
const colors = ['red', 'green', 'blue'];
const [firstColor, , thirdColor] = colors; // 'firstColor' = 'red', 'thirdColor' = 'blue'

// Destructuring avec valeurs par défaut
const { hobby = 'reading' } = person; // Si 'person.hobby' est indéfini, 'hobby' = 'reading'

// Destructuring dans les paramètres de fonction
function printPerson({ name, age }) {
  console.log(`${name} is ${age} years old.`);
}
printPerson(person); // Affiche : "Alice is 30 years old."
```

### Fonctionnement interne et cas d'utilisation

- Pour les **objets `{}`**, JavaScript recherche les **propriétés par nom** et les assigne aux variables correspondantes.
- Pour les **tableaux `[]`**, l'assignation se fait par **position**.
- Le destructuring peut être utilisé dans de nombreux contextes :
    - **Déclarations de variables** : Extraire facilement des données d'objets ou de tableaux.
    - **Paramètres de fonction** : Simplifier les signatures de fonctions en accédant directement aux propriétés nécessaires.
    - **Échanger des valeurs :**
    
    ```jsx
    let a = 1, b = 2;
    [a, b] = [b, a]; // a = 2, b = 1
    ```
    

### Cas pratiques

- **Données API** : Par exemple, pour extraire directement des valeurs d'une réponse JSON sans accéder à chaque propriété manuellement.
- **Fonctions avec plusieurs options** : Le destructuring permet d'extraire seulement les options requises d'un objet passé en paramètre.

```jsx
function setup({ width = 100, height = 200, color = 'blue' }) {
  console.log(`Setting up with width ${width}, height ${height}, color ${color}`);
}
```

## Spread et Rest Operators

L'opérateur **spread** (`...`) permet d'**étendre** (décomposer) un itérable, tandis que l'opérateur **rest** permet de **collecter** les éléments restants dans une fonction ou une déclaration de variable.

### Spread Operator (`...`)

```jsx
// Spread avec tableaux
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]

// Spread avec objets
const obj1 = { x: 1, y: 2 };
const obj2 = { ...obj1, z: 3 }; // { x: 1, y: 2, z: 3 }

// Copie superficielle d'objets
const originalObj = { a: 1, b: { c: 2 } };
const copyObj = { ...originalObj }; // Attention : 'b' est une référence, pas une copie profonde
```

### Rest Operator (`...`)

```jsx
// Rest dans les fonctions 
function sum(...numbers) {
  return numbers.reduce((acc, num) => acc + num, 0);
}
console.log(sum(1, 2, 3, 4)); // 10

// Rest dans la destructuration de tableau
const [first, ...others] = [1, 2, 3, 4]; // 'first' = 1, 'others' = [2, 3, 4]

// Rest dans la destructuration d'objet
const person = { name: 'Alice', age: 30, city: 'Paris' };
const { name, ...rest } = person; // 'name' = 'Alice', 'rest' = { age: 30, city: 'Paris' }
```

### Fonctionnement interne et cas d'utilisation

- **Spread** :
    - Le spread permet d'**étendre** un itérable en ses éléments constitutifs.
    - Fonctionne sur tous les objets **itérables** tels que les tableaux, les chaînes de caractères, les Sets, etc.
    - Utilisé pour :
        - **Fusionner des objets ou des tableaux**.
        - **Copier des objets/arrays** sans utiliser des méthodes complexes.
- **Rest** :
    - Le rest permet de **collecter** les arguments restants dans une fonction ou des éléments restants dans une opération de destructuration.
    - Particulièrement utile pour les fonctions qui acceptent un **nombre variable d'arguments**.

### Cas pratiques

- **Fonctions avec arguments variés** :
    
    ```jsx
    function multiply(multiplier, ...numbers) {
      return numbers.map(num => num * multiplier);
    }
    console.log(multiply(2, 1, 2, 3)); // [2, 4, 6]
    ```
    
- **Combinaison d'objets** :
    
    ```jsx
    const defaultConfig = { theme: 'dark', layout: 'grid' };
    const userConfig = { layout: 'list' };
    const finalConfig = { ...defaultConfig, ...userConfig }; // { theme: 'dark', layout: 'list' }
    ```
    
- **Copie d'objets** : Le spread effectue une **copie superficielle**, donc attention aux objets imbriqués qui resteront référencés

## C’est le moment de pratiquer 👨‍💻

### Exercice 1 : Création de la fonction `processUserData`

1. **Création d'une fonction `processUserData`** :
    - Utilise le **destructuring** pour extraire les propriétés `name` et `age` d’un **objet utilisateur**.
    - Retourne un **nouvel objet** contenant uniquement `name` et `age`. Les **autres propriétés** sont **ignorées**.

### Exercice 2 : Extension de `processUserData` avec une transformation

1. **Ajout d'une transformation de l'âge** :
    - Étend la fonction `processUserData` pour qu'elle prenne un second paramètre optionnel `transformAge`, qui est une fonction permettant de transformer l'âge.
    - Si aucun transformateur n'est fourni, l'âge est retourné tel quel.
    - Utilise une fonction `calculateBirthYear` pour transformer l'âge en année de naissance.

### Exercice 3 : Fusion des données avec `mergeUserData`

1. **Création de la fonction `mergeUserData`** :
    - Crée une fonction `mergeUserData` qui prend un nombre indéterminé d'objets utilisateur et les **fusionne** en utilisant le **spread operator**.
    - Les propriétés des **derniers objets** écrasent celles des précédents en cas de conflit.
    - Utilise `mergeUserData` pour combiner les objets utilisateurs traités avec `processUserData`.

### Challenge bonus : Validation des données utilisateur

1. **Validation des données fusionnées** :
    - Crée une fonction `isValidUser` pour **valider** les données utilisateur après la fusion.
    - Cette fonction vérifie que toutes les **propriétés essentielles** (`name`, `age`) sont présentes.

### Objectifs de l'exercice

Ces exercices t'aideront à :

- Pratiquer l'utilisation du **destructuring** pour extraire des données de manière efficace.
- Comprendre l'usage des **fonctions de transformation** pour la **manipulation flexible des données**.
- Développer des compétences en **fusion d'objets** avec le **spread operator** et l'utilisation de **`reduce`**.
- Introduire la **validation** des données, une étape essentielle lors de la manipulation de données en production.