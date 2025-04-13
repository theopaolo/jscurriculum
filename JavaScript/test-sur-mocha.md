# Test sur Mocha

# Introduction

### Qu'est-ce que Mocha ?

Mocha est un framework de test JavaScript flexible qui :

- Fonctionne dans Node.js et le navigateur
- Supporte le test asynchrone
- Offre une syntaxe BDD/TDD
- Permet des rapports précis et personnalisables
- S'intègre avec de nombreuses bibliothèques d'assertions (Chai, Should.js, etc.)

### Écosystème Mocha

- 🔍 Mocha : Framework de test
- ✅ Chai : Bibliothèque d'assertions
- 🕵️ Sinon : Mocks, stubs et espions
- 📊 Istanbul/NYC : Couverture de code

# Configuration et Installation

### Installation de base

```bash
# Installation des dépendances principales
npm install --save-dev mocha chai sinon

# Installation optionnelle pour la couverture de code
npm install --save-dev nyc
```

### Configuration package.json

```json
{
  "scripts": {
    "test": "mocha 'test/**/*.test.js'",
    "test:watch": "mocha --watch 'test/**/*.test.js'",
    "test:coverage": "nyc mocha 'test/**/*.test.js'"
  },
  "mocha": {
    "timeout": 5000,
    "recursive": true,
    "exit": true
  }
}
```

# Structure des Tests

### Organisation des fichiers

```
project/
├── src/
│   └── calculator.js
└── test/
    ├── unit/
    │   └── calculator.test.js
    └── integration/
        └── api.test.js

```

### Structure de base

```jsx
// calculator.js
class Calculator {
  add(a, b) { return a + b; }
  subtract(a, b) { return a - b; }
}

module.exports = Calculator;

// calculator.test.js
const { expect } = require('chai');
const Calculator = require('../src/calculator');

describe('Calculator', () => {
  let calculator;

  beforeEach(() => {
    calculator = new Calculator();
  });

  describe('add', () => {
    it('should add two positive numbers', () => {
      expect(calculator.add(2, 3)).to.equal(5);
    });

    it('should handle negative numbers', () => {
      expect(calculator.add(-2, 3)).to.equal(1);
    });
  });
});
```

# Assertions avec Chai

### Styles d'assertions

```jsx
const { expect, should, assert } = require('chai');
should(); // Activer le style should

describe('Styles d\\'assertions Chai', () => {
  // Style expect
  it('using expect style', () => {
    expect(2 + 2).to.equal(4);
    expect([1, 2, 3]).to.have.lengthOf(3);
    expect({ name: 'test' }).to.have.property('name');
  });

  // Style should
  it('using should style', () => {
    (2 + 2).should.equal(4);
    [1, 2, 3].should.have.lengthOf(3);
    ({ name: 'test' }).should.have.property('name');
  });

  // Style assert
  it('using assert style', () => {
    assert.equal(2 + 2, 4);
    assert.lengthOf([1, 2, 3], 3);
    assert.property({ name: 'test' }, 'name');
  });
});
```

### Assertions communes

```jsx
describe('Chai Assertions Examples', () => {
  it('démonstration des assertions courantes', () => {
    // Égalité
    expect(2 + 2).to.equal(4);
    expect({ a: 1 }).to.deep.equal({ a: 1 });

    // Types
    expect(42).to.be.a('number');
    expect('test').to.be.a('string');
    expect([]).to.be.an('array');

    // Comparaisons
    expect(5).to.be.above(3);
    expect(3).to.be.below(5);
    expect(4).to.be.within(1, 5);

    // Arrays
    expect([1, 2, 3]).to.include(2);
    expect([1, 2, 3]).to.have.lengthOf(3);

    // Objets
    expect({ a: 1, b: 2 }).to.have.property('a');
    expect({ a: 1, b: 2 }).to.include({ a: 1 });

    // Chaînes
    expect('hello world').to.match(/^hello/);
    expect('hello').to.have.lengthOf(5);
  });
});

```

# Tests Asynchrones

### Promesses

```jsx
const axios = require('axios');

describe('Tests Asynchrones', () => {
  // Avec Promise
  it('test avec Promise', () => {
    return axios.get('<https://api.example.com/data>')
      .then(response => {
        expect(response.data).to.have.property('success');
      });
  });

  // Avec async/await
  it('test avec async/await', async () => {
    const response = await axios.get('<https://api.example.com/data>');
    expect(response.data).to.have.property('success');
  });

  // Gestion des erreurs
  it('test des erreurs async', async () => {
    try {
      await axios.get('<https://invalid-url>');
    } catch (error) {
      expect(error).to.be.an('error');
    }
  });
});
```

### Callbacks

```jsx
describe('Tests avec Callbacks', () => {
  it('devrait gérer les callbacks', (done) => {
    function asyncOperation(callback) {
      setTimeout(() => callback(null, 'success'), 100);
    }

    asyncOperation((err, result) => {
      try {
        expect(err).to.be.null;
        expect(result).to.equal('success');
        done();
      } catch (error) {
        done(error);
      }
    });
  });
});

```

# Hooks et Fixtures

### Utilisation des hooks

```jsx
describe('Database Operations', () => {
  let db;

  before(async () => {
    // Setup une fois avant tous les tests
    db = await initDatabase();
  });

  beforeEach(async () => {
    // Reset avant chaque test
    await db.clear();
  });

  afterEach(async () => {
    // Cleanup après chaque test
    await db.reset();
  });

  after(async () => {
    // Cleanup final
    await db.close();
  });

  it('should insert data correctly', async () => {
    const result = await db.insert({ data: 'test' });
    expect(result).to.have.property('id');
  });
});
```

# Mocks avec Sinon

### Types de mocks

```jsx
const sinon = require('sinon');

describe('Sinon Examples', () => {
  // Spies
  it('using spy', () => {
    const callback = sinon.spy();
    [1, 2, 3].forEach(callback);
    expect(callback.callCount).to.equal(3);
  });

  // Stubs
  it('using stub', () => {
    const stub = sinon.stub();
    stub.returns(42);
    expect(stub()).to.equal(42);
  });

  // Mocks
  it('using mock', () => {
    const mock = sinon.mock(axios);
    mock.expects('get').once().returns(Promise.resolve({ data: 'test' }));

    return axios.get('/test')
      .then(response => {
        expect(response.data).to.equal('test');
        mock.verify();
      });
  });
});
```

### Exemple complet avec Sinon

```jsx
describe('UserService avec Sinon', () => {
  let userService;
  let dbStub;

  beforeEach(() => {
    // Créer un stub pour la base de données
    dbStub = {
      query: sinon.stub()
    };
    userService = new UserService(dbStub);
  });

  afterEach(() => {
    // Restaurer tous les stubs
    sinon.restore();
  });

  it('should get user by id', async () => {
    // Arrange
    const expectedUser = { id: 1, name: 'John' };
    dbStub.query.withArgs('SELECT * FROM users WHERE id = ?', [1])
      .resolves([expectedUser]);

    // Act
    const user = await userService.getUserById(1);

    // Assert
    expect(user).to.deep.equal(expectedUser);
    expect(dbStub.query).to.have.been.calledOnce;
  });
});
```

# Exemples Pratiques

### Test d'API REST

```jsx
const request = require('supertest');
const app = require('../src/app');

describe('API Tests', () => {
  describe('GET /api/users', () => {
    it('should return users list', async () => {
      const response = await request(app)
        .get('/api/users')
        .expect('Content-Type', /json/)
        .expect(200);

      expect(response.body).to.be.an('array');
      expect(response.body[0]).to.have.property('id');
    });
  });

  describe('POST /api/users', () => {
    it('should create new user', async () => {
      const userData = {
        name: 'John',
        email: 'john@test.com'
      };

      const response = await request(app)
        .post('/api/users')
        .send(userData)
        .expect(201);

      expect(response.body).to.include(userData);
    });
  });
});
```

# Configuration Avancée

### .mocharc.js

```jsx
module.exports = {
  // Options de base
  recursive: true,
  exit: true,
  timeout: 5000,

  // Reporter
  reporter: 'spec',
  reporterOptions: {
    colors: true
  },

  // Fichiers
  spec: ['test/**/*.test.js'],

  // Require files
  require: [
    'chai/register-expect',
    './test/setup.js'
  ],

  // Watch options
  watch: false,
  'watch-files': ['src/**/*.js', 'test/**/*.js'],

  // Coverage avec NYC
  coverage: false,
  'coverage-reporter': ['text', 'html']
};
```

### Scripts NPM avancés

```json
{
  "scripts": {
    "test": "mocha",
    "test:watch": "mocha --watch",
    "test:coverage": "nyc mocha",
    "test:coverage:report": "nyc --reporter=html --reporter=text mocha",
    "test:debug": "mocha --inspect-brk",
    "test:ci": "mocha --reporter mocha-junit-reporter"
  }
}

```

# Best Practices

### Structure recommandée

```jsx
describe('UserService', () => {
  // Configuration commune
  const sandbox = sinon.createSandbox();
  let userService;
  let dbStub;

  beforeEach(() => {
    // Reset pour chaque test
    dbStub = {
      query: sandbox.stub()
    };
    userService = new UserService(dbStub);
  });

  afterEach(() => {
    // Cleanup
    sandbox.restore();
  });

  // Tests groupés par fonctionnalité
  describe('#createUser', () => {
    it('devrait créer un utilisateur valide', async () => {
      // Arrange
      const userData = { name: 'Test User', email: 'test@example.com' };
      dbStub.query.resolves({ insertId: 1 });

      // Act
      const result = await userService.createUser(userData);

      // Assert
      expect(result).to.have.property('id', 1);
      expect(dbStub.query).to.have.been.calledOnce;
    });

    it('devrait gérer les erreurs de validation', async () => {
      // Arrange
      const invalidData = { name: '' };

      // Act & Assert
      await expect(userService.createUser(invalidData))
        .to.be.rejectedWith('Validation error');
    });
  });
});

```

### Tips importants

1. Utilisez des noms de tests descriptifs
2. Suivez le pattern AAA (Arrange-Act-Assert)
3. Isolez vos tests
4. Utilisez des mocks appropriés
5. Gérez correctement les opérations asynchrones
6. Nettoyez après chaque test
7. Groupez les tests logiquement
8. Évitez les dépendances entre tests
9. Maintenez la cohérence des assertions
10. Documentez les cas particuliers