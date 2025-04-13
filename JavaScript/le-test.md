# Le test avec Jest

## Deux des principes de développement test.

Jest utilise une approche  BDD.

![BDD vs TDD.png](Le%20test%20avec%20Jest%2012a3324cc3d780b48aafe63e23706637/BDD_vs_TDD.png)

https://www.geeksforgeeks.org/difference-between-bdd-vs-tdd-in-software-engineering/

# Introduction

Jest est un framework de test JavaScript complet et simple créé par Facebook. Il est particulièrement populaire pour tester les applications React, mais il peut être utilisé pour tout projet JavaScript. Jest est apprécié pour sa simplicité d’utilisation, sa rapidité et ses fonctionnalités intégrées comme le mocking, la surveillance de fonctions (spying), les tests asynchrones et la génération de rapports de couverture de code.

### **Pourquoi utiliser Jest ?**

- 🔧  **Configuration minimale** : Jest fonctionne sans configuration supplémentaire pour la plupart des projets JavaScript.
- ⚡ **Rapide** : Grâce à sa capacité à exécuter des tests en parallèle, Jest est optimisé pour la vitesse.
- 📦 **Fonctionnalités intégrées** : Il inclut le support du mocking, du coverage, et des snapshots, sans avoir besoin d’installer des bibliothèques supplémentaires.
- **🤝 Communauté active** : Jest est largement adopté, ce qui signifie qu’il y a beaucoup de ressources et de support disponibles.

# Configuration et Installation

### Installation de base

```bash
# Avec npm
npm install --save-dev jest

# Avec yarn
yarn add --dev jest
```

### Configuration package.json

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:debug": "jest --runInBand --detectOpenHandles"
  },
  "jest": {
    "testEnvironment": "node",
    "coverageThreshold": {
      "global": {
        "branches": 80,
        "functions": 80,
        "lines": 80
      }
    }
  }
}

// Cela vous permet d’exécuter tous les tests avec npm test ou yarn test, et d’exécuter Jest en mode veille avec npm run test:watch ou yarn test:watch.
```

# Structure des Tests

<aside>
<img src="https://www.notion.so/icons/info-alternate_green.svg" alt="https://www.notion.so/icons/info-alternate_green.svg" width="40px" />

Jest utilise une approche de test basée sur les comportements (Behavior-Driven Development - BDD) avec les fonctions describe, test et expect.

</aside>

**Blocs de Tests**

- `describe(name, fn)`: Regroupe des tests liés sous un même nom, permettant une organisation hiérarchique.
- `test(name, fn)` ou `it(name, fn)`: Définit un cas de test. it est un alias de test et peut être utilisé pour améliorer la lisibilité.

**Fonctions de Configuration**

`beforeAll(fn)`: Exécuté une fois avant tous les tests du bloc describe.

`afterAll(fn)`: Exécuté une fois après tous les tests du bloc describe.

`beforeEach(fn)`: Exécuté avant chaque test du bloc describe.

`afterEach(fn)`: Exécuté après chaque test du bloc describe.

### Convention de nommage

```
project/
├── src/
│   └── math.js
└── __tests__/
    └── math.test.js  // ou math.spec.js

```

### **Exemple de Structure**

```jsx
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => a - b;

// math.test.js
import { add, subtract } from '../src/math';

describe('Math Operations', () => {
  describe('add', () => {
    test('adds two positive numbers', () => {
      expect(add(2, 3)).toBe(5);
    });

    test('handles negative numbers', () => {
      expect(add(-2, 3)).toBe(1);
    });
  });

  describe('subtract', () => {
    // Tests pour subtract...
  });
});
```

### Hooks de test

```jsx
describe('Suite de tests pour la fonction addition', () => {
  beforeAll(() => {
    // Exécuté une fois avant tous les tests
  });

  beforeEach(() => {
    // Exécuté avant chaque test
  });

  afterEach(() => {
    // Exécuté après chaque test
  });

  afterAll(() => {
    // Exécuté une fois après tous les tests
  });

  test('doit additionner deux nombres positifs', () => {
    expect(add(2, 3)).toBe(5);
  });

  it('doit retourner 0 si les deux nombres sont 0', () => {
    expect(add(0, 0)).toBe(0);
  });
});
```

# Les Matchers

Les matchers sont des méthodes utilisées avec expect pour valider le résultat attendu.

### **Égalité**

- `toBe(value)`: Vérifie que la valeur est strictement égale (===) à value.
- `toEqual(value)`: Vérifie que la valeur est égale à value. Pour les objets et les tableaux, cela vérifie que les valeurs sont égales en contenu.
- `toStrictEqual(value)`: Comme toEqual, mais vérifie également que les objets n’ont pas de propriétés inattendues.

```jsx
expect(2 + 2).toBe(4);
expect({ a: 1 }).toEqual({ a: 1 });
```

### **Vérifications de Type et de Valeur**

- `toBeNull():` Vérifie que la valeur est null.
- `toBeUndefined()`: Vérifie que la valeur est undefined.
- `toBeDefined()`: Vérifie que la valeur n’est pas undefined.
- `toBeTruthy()`: Vérifie que la valeur est évaluée comme vraie.
- `toBeFalsy()`: Vérifie que la valeur est évaluée comme fausse

```jsx
expect(null).toBeNull();
expect(undefined).toBeUndefined();
expect(1).toBeDefined();
expect(true).toBeTruthy();
expect(false).toBeFalsy();
```

## **Nombres**

- `toBeGreaterThan(number)`: Vérifie que la valeur est supérieure à number.
- `toBeGreaterThanOrEqual(number)`: Vérifie que la valeur est supérieure ou égale à number.
- `toBeLessThan(number)`: Vérifie que la valeur est inférieure à number.
- `toBeLessThanOrEqual(number)`: Vérifie que la valeur est inférieure ou égale à number.
- `toBeCloseTo(number, numDigits)`: Vérifie que la valeur est proche de number, utile pour les nombres à virgule flottante.

```jsx
expect(10).toBeGreaterThan(5);
expect(5).toBeLessThanOrEqual(5);
expect(0.1 + 0.2).toBeCloseTo(0.3, 5);
```

### **Chaînes de Caractères**

- `toMatch(regexpOrString)`: Vérifie que la chaîne correspond à l’expression régulière ou contient la sous-chaîne spécifiée.

```jsx
expect('Hello World').toMatch(/World/);
expect('JavaScript').toMatch('Script');
```

### **Tableaux et Itérables**

- `toContain(item)`: Vérifie que l’itérable contient l’élément spécifié.
- `toContainEqual(item)`: Pour les tableaux d’objets, vérifie qu’un objet égal à item est présent.
- `toHaveLength(number)`: Vérifie que l’itérable a la longueur spécifiée.

```jsx
expect([1, 2, 3]).toContain(2);
expect([{ a: 1 }]).toContainEqual({ a: 1 });
expect('Hello').toHaveLength(5);
```

### **Objets**

- `toHaveProperty(keyPath, value?)`: Vérifie que l’objet possède la propriété spécifiée, avec une valeur optionnelle.

```jsx
const obj = { a: { b: 2 } };
expect(obj).toHaveProperty('a.b', 2);
```

### Matchers Essentiels avec Exemples

```jsx
// Égalité et Comparaison
test('démonstration des matchers de base', () => {
  const value = 2 + 2;
  const obj = { name: 'test' };
  const arr = [1, 2, 3];

  // Égalité
  expect(value).toBe(4);                    // Égalité stricte
  expect(obj).toEqual({ name: 'test' });    // Égalité profonde

  // Comparaisons numériques
  expect(value).toBeGreaterThan(3);
  expect(value).toBeLessThanOrEqual(4);
  expect(0.1 + 0.2).toBeCloseTo(0.3, 5);   // Pour les nombres flottants

  // Vérifications booléennes
  expect(true).toBeTruthy();
  expect(false).toBeFalsy();
  expect(null).toBeNull();
  expect(undefined).toBeUndefined();

  // Arrays et objets
  expect(arr).toContain(2);
  expect(obj).toHaveProperty('name');
  expect(arr).toHaveLength(3);
});
```

### Matchers Personnalisés

```jsx
expect.extend({
  toBeValidUser(received) {
    const pass = received.name && received.email;
    return {
      pass,
      message: () => `expected ${received} to be a valid user`,
    };
  },
});

test('user validation', () => {
  const user = { name: 'John', email: 'john@example.com' };
  expect(user).toBeValidUser();
});
```

# Tests Asynchrones

Tester du code asynchrone nécessite une approche différente pour s’assurer que Jest attend la fin des opérations asynchrones.

### Promesses

**Avec return**

En retournant la promesse, Jest attendra qu’elle soit résolue ou rejetée.

```jsx
test('résolution de la promesse', () => {
  return fetchData().then(data => {
    expect(data).toBe('données');
  });
});
```

**Avec async/await**

Utilisez les fonctions asynchrones pour un code plus lisible.

```jsx
test('résolution de la promesse avec async/await', async () => {
  const data = await fetchData();
  expect(data).toBe('données');
});
```

**Tester les rejets**

Utilisez catch ou rejects.

```jsx
test('rejet de la promesse', () => {
  return fetchData().catch(e => expect(e).toMatch('erreur'));
});

// Ou avec async/await
test('rejet de la promesse avec async/await', async () => {
  await expect(fetchData()).rejects.toMatch('erreur');
});
```

### Exemple avec un API

```jsx
// Service API
const userService = {
  async getUser(id) {
    // Simulation d'appel API
    return Promise.resolve({ id, name: 'John' });
  }
};

// Tests
describe('UserService', () => {
  test('getUser retourne les données utilisateur', async () => {
    const user = await userService.getUser(1);
    expect(user).toEqual({ id: 1, name: 'John' });
  });

  test('getUser avec gestion d\\'erreur', async () => {
    await expect(userService.getUser(-1))
      .rejects
      .toThrow('Invalid ID');
  });
});
```

### Callbacks

Si la fonction asynchrone utilise des callbacks, utilisez le paramètre done pour indiquer à Jest quand le test est terminé.

```jsx
test('callback test', done => {
  function fetchData(callback) {
    setTimeout(() => callback(null, 'Done'), 100);
  }

  fetchData((err, data) => {
    try {
      expect(err).toBeNull();
      expect(data).toBe('Done');
      done();
    } catch (error) {
      done(error);
    }
  });
});
```

# **Mocks et Espions (Spies)**

Les mocks vous permettent de simuler le comportement de fonctions ou de modules pour isoler le code à tester.

### Function Mocks

**Création d’une fonction mock**

- `jest.fn(implementation?)`: Crée une fonction mock facultativement avec une implémentation.

```jsx
const mockFn = jest.fn();

// Avec une implémentation
const mockAdd = jest.fn((a, b) => a + b);
```

### **Configurer les valeurs de retour**

- `mockFn.mockReturnValue(value)`: Définit la valeur de retour.
- `mockFn.mockResolvedValue(value)`: Pour les fonctions asynchrones, définit une valeur de retour résolue.
- `mockFn.mockRejectedValue(value)`: Pour les fonctions asynchrones, définit une valeur de retour rejetée.

```jsx
mockFn.mockReturnValue('valeur');
mockFn.mockResolvedValue('valeur asynchrone');
mockFn.mockRejectedValue(new Error('erreur'));
```

## **Vérifications sur les fonctions mock**

```jsx
describe('Mock Examples', () => {
  test('mock function basic', () => {
    const mockFn = jest.fn();

    // Configuration du mock
    mockFn.mockReturnValue('default');
    mockFn.mockReturnValueOnce('first call');

    // Utilisation
    expect(mockFn()).toBe('first call');
    expect(mockFn()).toBe('default');

    // Vérifications
    expect(mockFn).toHaveBeenCalledTimes(2);
  });

  test('mock implementation', () => {
    const mockFn = jest.fn((a, b) => a + b);

    expect(mockFn(2, 3)).toBe(5);
    expect(mockFn).toHaveBeenCalledWith(2, 3);
  });
});
```

### Module Mocking

```jsx
// users.js
import axios from 'axios';

export const getUsers = async () => {
  const { data } = await axios.get('/api/users');
  return data;
};

// users.test.js
jest.mock('axios');

import axios from 'axios';
import { getUsers } from './users';

test('mock axios get', async () => {
  const users = [{ id: 1, name: 'John' }];
  axios.get.mockResolvedValue({ data: users });

  const result = await getUsers();
  expect(result).toEqual(users);
  expect(axios.get).toHaveBeenCalledWith('/api/users');
});
```

# Exemples Pratiques

### Test d'une API REST

```jsx
import request from 'supertest';
import app from './app';

describe('API Tests', () => {
  describe('GET /api/users', () => {
    it('retourne la liste des utilisateurs', async () => {
      const response = await request(app)
        .get('/api/users')
        .expect('Content-Type', /json/)
        .expect(200);

      expect(response.body).toEqual(
        expect.arrayContaining([
          expect.objectContaining({
            id: expect.any(Number),
            name: expect.any(String)
          })
        ])
      );
    });
  });

  describe('POST /api/users', () => {
    it('crée un nouvel utilisateur', async () => {
      const userData = { name: 'John', email: 'john@test.com' };

      const response = await request(app)
        .post('/api/users')
        .send(userData)
        .expect(201);

      expect(response.body).toMatchObject(userData);
    });
  });
});
```

# Best Practices

### Pattern AAA (Arrange-Act-Assert)

```jsx
describe('UserService', () => {
  test('createUser with valid data', async () => {
    // Arrange
    const userData = {
      name: 'John Doe',
      email: 'john@example.com'
    };
    const userService = new UserService();

    // Act
    const user = await userService.createUser(userData);

    // Assert
    expect(user).toMatchObject({
      id: expect.any(Number),
      ...userData,
      createdAt: expect.any(Date)
    });
  });
});

```

### Tests Isolés

```jsx
describe('ShoppingCart', () => {
  let cart;

  beforeEach(() => {
    cart = new ShoppingCart();
  });

  test('addItem increases total correctly', () => {
    cart.addItem({ price: 10 });
    expect(cart.total).toBe(10);
  });

  test('removeItem decreases total correctly', () => {
    cart.addItem({ price: 10 });
    cart.removeItem({ price: 10 });
    expect(cart.total).toBe(0);
  });
});
```

# Configuration Avancée

### jest.config.js Complet

```jsx
module.exports = {
  // Environnement de test
  testEnvironment: 'node',

  // Couverture de code
  collectCoverageFrom: [
    'src/**/*.{js,jsx}',
    '!**/node_modules/**',
    '!**/vendor/**'
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  },

  // Configuration des chemins
  moduleNameMapper: {
    '^@/(.*)$': '<rootDir>/src/$1'
  },

  // Setup & Teardown
  setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],

  // Timeouts
  testTimeout: 10000,

  // Reporter personnalisé
  reporters: [
    'default',
    ['jest-junit', {
      outputDirectory: 'reports',
      outputName: 'jest-junit.xml',
    }]
  ]
};
```

# Débogage et Outils

### Debugging avec VS Code

```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug Jest Tests",
      "program": "${workspaceFolder}/node_modules/.bin/jest",
      "args": ["--runInBand", "--watchAll=false"],
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    }
  ]
}
```

### Scripts NPM Utiles

```json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:coverage": "jest --coverage",
    "test:debug": "jest --runInBand --detectOpenHandles",
    "test:ci": "jest --ci --reporters='default' --reporters='jest-junit'",
    "test:clear": "jest --clearCache"
  }
}
```