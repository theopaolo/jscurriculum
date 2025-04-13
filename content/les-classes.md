# Les Classes

<aside>
## Définition

Une **classe** est un plan de construction (*blueprint*) qui **définit la structure** et le comportement d'un objet en encapsulant ses **propriétés et méthodes**. Elle permet de créer des instances d'objets avec leurs propres états et comportements, tout en maintenant une séparation claire entre les éléments publics et privés grâce au système de visibilité natif.
</aside>

### Concept clé

Les classes permettent de créer des objets structurés avec un état interne et des comportements définis. Ce mécanisme encapsule les données et les méthodes dans une seule entité, permettant la création de multiples instances partageant le même comportement tout en maintenant leur propre état.

- Une classe peut définir des propriétés et méthodes publiques et privées avec une syntaxe claire.
- Les champs privés sont explicitement marqués avec le préfixe `#`.

### Comparaison avec l'Approche par Closures

Les deux approches permettent l'encapsulation, mais avec des différences importantes :

Une alternative aux closures est l'utilisation des classes en JavaScript. Voici comment les mêmes concepts peuvent être implémentés en utilisant la programmation orientée objet (POO).

### Exemples

```jsx
class Counter {
  #count = 0;  // Propriété privée

  increment() {
    this.#count++;
    return this.#count;
  }

  getCount() {
    return this.#count;
  }
}

const counter = new Counter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.#count); // SyntaxError: Private field '#count' must be declared in an enclosing class
```

```jsx
class BankAccount {
  #balance = 0;  // Propriété privée

  deposit(amount) {
    if (amount > 0) {
      this.#balance += amount;
      return true;
    }
    return false;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount();
account.deposit(100);
console.log(account.getBalance()); // 100
console.log(account.#balance); // SyntaxError: Private field
```

### Fonctionnement interne et implications

- Chaque classe crée un **prototype** qui est partagé entre toutes les instances.
- Les propriétés privées sont strictement encapsulées grâce au préfixe `#`.
- L'héritage est géré de manière native via le mot-clé `extends`.
- La création d'instances se fait via le mot-clé `new`.

## Applications courantes

Les classes sont particulièrement utiles dans plusieurs contextes :

- **Modélisation d'objets métier** : Création de structures complexes avec comportements.
- **Composants réutilisables** : Base pour des éléments d'interface utilisateur.
- **Hiérarchies d'objets** : Organisation de code via l'héritage.
- **APIs publiques** : Interfaces claires avec encapsulation stricte.

## Exemple : Système de gestion utilisateur

```jsx
class UserManager {
  #users = new Map();
  #currentId = 0;

  addUser(name, email) {
    const id = this.#generateId();
    this.#users.set(id, { name, email });
    return id;
  }

  getUser(id) {
    return this.#users.get(id);
  }

  #generateId() {
    return this.#currentId++;
  }
}

const userManager = new UserManager();
const userId = userManager.addUser("John", "john@example.com");
```

## Avantages des Classes

- **Syntaxe claire** : Structure plus explicite et familière.
- **Encapsulation native** : Support intégré des champs privés.
- **Meilleures performances** : Optimisation du moteur JavaScript.
- **Support de l'héritage** : Mécanisme natif avec `extends`.
- **Intégration TypeScript** : Support excellent du typage.

## Attention !

- **Piège du this** : Le contexte peut être perdu dans certaines situations
- **Complexité** : L'héritage profond peut créer des dépendances complexes
- **Taille du bundle** : Les classes peuvent augmenter la taille du code final

### Exemple de piège courant

```jsx
class Timer {
  #seconds = 0;

  start() {
    // Problème : 'this' est perdu
    setInterval(function() {
      this.#seconds++; // Erreur!
    }, 1000);
  }

  // Solution : utiliser une arrow function
  betterStart() {
    setInterval(() => {
      this.#seconds++;
    }, 1000);
  }
}
```

## Bonnes pratiques

- Utilisez des noms de classe en **PascalCase**
- Préférez la **composition** à l'héritage quand possible
- Gardez les classes **focalisées** sur une seule responsabilité
- Documentez l'interface publique de vos classes

### Exercices de Comparaison

Reprenons les exercices précédents en utilisant l'approche orientée objet :

### Exercice 1 : Validation de mot de passe avec Classe

```jsx
class PasswordValidator {
  #regex;

  constructor(regex) {
    this.#regex = regex;
  }

  validate(password) {
    return this.#regex.test(password);
  }
}

const validator = new PasswordValidator(/^(?=.*\\d)(?=.*[a-z])(?=.*[A-Z]).{8,}$/);
console.log(validator.validate("Password123")); // true

```

### Exercice 2 : Compte Bancaire avec Classe

```jsx
class BankAccount {
  #balance = 0;

  deposit(amount) {
    if (amount > 0) {
      this.#balance += amount;
      return true;
    }
    return false;
  }

  withdraw(amount) {
    if (amount > 0 && this.#balance >= amount) {
      this.#balance -= amount;
      return true;
    }
    return false;
  }

  getBalance() {
    return this.#balance;
  }
}

const account = new BankAccount();
account.deposit(100);
console.log(account.getBalance()); // 100

```

### Exercice 3 : Gestionnaire de Tâches avec Classe

```jsx
class TaskManager {
  #tasks = {
    pending: [],
    completed: []
  };

  addTask(task) {
    this.#tasks.pending.push(task);
  }

  completeTask(task) {
    const index = this.#tasks.pending.indexOf(task);
    if (index !== -1) {
      this.#tasks.pending.splice(index, 1);
      this.#tasks.completed.push(task);
    }
  }

  getTasks() {
    return {
      pending: [...this.#tasks.pending],
      completed: [...this.#tasks.completed]
    };
  }
}

const manager = new TaskManager();
manager.addTask("Learn Classes");
manager.completeTask("Learn Classes");
console.log(manager.getTasks());

```

### Quand Utiliser Quoi ?

1. **Utilisez les Classes quand :**
    - Vous avez besoin d'une hiérarchie d'objets (héritage)
    - La structure de vos données est complexe
    - Vous travaillez avec TypeScript
    - Vous voulez une syntaxe plus explicite
2. **Utilisez les Closures quand :**
    - Vous avez besoin d'une encapsulation simple
    - Vous créez des fonctions d'ordre supérieur
    - Vous travaillez avec des callbacks
    - Vous voulez maintenir la compatibilité avec d'ancien code

### Conclusion

Les classes fournissent une syntaxe moderne et structurée pour l'organisation du code en JavaScript. Elles offrent une encapsulation plus stricte que les closures et sont particulièrement adaptées aux applications complexes nécessitant une modélisation claire des objets et de leurs relations. Cependant, il est important de les utiliser judicieusement en considérant les besoins spécifiques du projet.