# Express

# Introduction à Express.js

## Qu'est-ce qu'Express.js ?

Express.js est un framework web minimaliste et flexible pour Node.js, qui permet de créer rapidement des applications web et des API. Il fournit un ensemble d'outils puissants tout en restant simple à utiliser, ce qui en fait le framework web le plus populaire pour Node.js.

<aside>
💡

Remarque : Express.js ne gère pas tout à votre place. Il vous donne juste les outils essentiels pour démarrer rapidement, en vous laissant la liberté de structurer vos projets comme vous le souhaitez.

</aside>

## Pourquoi utiliser Express.js ?

1. **Simplicité** : Une API simple et intuitive qui facilite la prise en main.
2. **Flexibilité** : Express ne vous impose pas de structure spécifique pour vos projets.
3. **Middleware** : Possibilité d'ajouter facilement des fonctionnalités via des middlewares.
4. **Performance** : Léger et rapide, Express minimise les surcharges inutiles.
5. **Écosystème riche** : Large collection de middlewares et d'extensions disponibles.
6. **Support TypeScript** : Excellent support de TypeScript pour le développement moderne.
7. **Documentation extensive** : Une documentation complète et une grande communauté

## Installation et Configuration Initiale

```bash
mkdir mon-projet-express
cd mon-projet-express
npm init -y               # Initialiser un projet Node.js
npm install express       # Installer Express
npm install nodemon -D    # Installer nodemon en dépendance de développement
```

Dans votre package json vous pouvez ajouter les scripts suivant :

```bash
{
  "scripts": {
    "dev": "nodemon app.js",
    "start": "node app.js"
  }
}
```

<aside>
💡

**Astuce** : Si vous commencez avec Node.js, utilisez aussi `nodemon` pour redémarrer automatiquement le serveur à chaque modification :

</aside>

## Concepts de Base

### 1. Création d'un serveur Express basique

Le code suivant illustre la création d'un serveur minimal qui renvoie "Hello World" à la racine `(/)`.

```jsx
const express = require('express');
const app = express();
const port = 3000;

// Middleware pour parser le JSON
app.use(express.json());

// Middleware de logging basique
app.use((req, res, next) => {
  console.log(`${new Date().toISOString()} - ${req.method} ${req.url}`);
  next();
});

// Route de base
app.get('/', (req, res) => {
  res.json({ message: 'Bienvenue sur l\'API!' });
});

// Gestion des erreurs 404
app.use((req, res) => {
  res.status(404).json({ error: 'Route non trouvée' });
});

// Démarrage du serveur
app.listen(port, () => {
  console.log(`🚀 Serveur démarré sur http://localhost:${port}`);
});
```

### 2. Routing

Le **routing** permet de définir comment votre serveur répond aux requêtes HTTP à différentes URL.

```jsx
// Groupement de routes avec des préfixes
const router = express.Router();

// Middleware spécifique aux routes
router.use((req, res, next) => {
  console.log('Time:', Date.now());
  next();
});

// Routes avec validation des paramètres
router.get('/users/:id', (req, res, next) => {
  const id = parseInt(req.params.id);
  if (isNaN(id)) {
    return next(new Error('ID invalide'));
  }
  res.json({ id });
});

// Support des promesses et async/await
router.get('/async', async (req, res) => {
  try {
    const data = await fetchSomeData();
    res.json(data);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.use('/api', router);
```

<aside>
💡

À savoir : Vous pouvez utiliser des expressions régulières ou des fonctions complexes pour gérer des routes plus dynamiques.

</aside>

### 3. Middleware

Un **middleware** est une fonction exécutée entre la requête et la réponse. Il peut modifier la requête, la réponse, ou même arrêter le traitement si nécessaire. Il ont accès access au objets `res` `req` et `next`

```jsx
const express = require('express');
const app = express();

// Middleware simple
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
});

// Middleware pour une route spécifique
app.use('/api', (req, res, next) => {
  // Vérifier l'authentification
  next();
});

// Middleware de gestion d'erreurs
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).send('Something broke!');
});
```

*Ajoute un middleware pour logger chaque requête dans le serveur et observe les résultats dans la console.*

### 4. Servir des fichiers statiques

Pour servir des fichiers comme des images ou des fichiers CSS/JS, utilisez `express`.`static`.

```jsx
// Servir des fichiers depuis le dossier 'public'
app.use(express.static('public'));

// Avec un préfixe
app.use('/static', express.static('public'));
```

### 5. Gestion des requêtes et réponses

Express simplifie la gestion des requêtes et réponses avec des outils pour parser des données JSON ou des formulaires.

```jsx
// Parser le JSON
app.use(express.json());

// Parser les données de formulaire
app.use(express.urlencoded({ extended: true }));

// Exemple de traitement
app.post('/api/data', (req, res) => {
  const data = req.body;  // Données JSON ou formulaire
  res.json({
    received: data,
    status: 'success'
  });
});
```

### 6. Organisation des routes

Organiser vos routes en différents fichiers permet de structurer votre projet de manière plus lisible.

```jsx
// userRoutes.js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.send('Users list');
});

router.post('/', (req, res) => {
  res.send('Create user');
});

module.exports = router;

// app.js
const userRoutes = require('./routes/userRoutes');
app.use('/users', userRoutes);
```

## Middlewares Courants

Voici quelques middlewares très utilisés dans les applications Express :

1. **Morgan** - Pour le logging des requêtes HTTP

```jsx
const morgan = require('morgan');
app.use(morgan('dev'));
```

1. **CORS** - Pour gérer les requêtes Cross-Origin Resource Sharing 

```jsx
const cors = require('cors');
app.use(cors());
```

1. **Helmet** - Pour renforcer la sécurité de vos applications HTTP

```jsx
const helmet = require('helmet');
app.use(helmet());
```

## Gestion des Erreurs

Il est essentiel d'ajouter un middleware de gestion des erreurs pour capturer et traiter les exceptions.

```jsx
// Middleware de gestion d'erreur personnalisé
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(err.status || 500).json({
    error: {
      message: err.message || 'Internal Server Error',
      status: err.status || 500
    }
  });
});

// Lancer une erreur
app.get('/error', (req, res, next) => {
  const err = new Error('Something went wrong');
  err.status = 400;
  next(err);
});
```

***Mini exo** : Ajoute une route /error qui lance une erreur avec next(err) et vérifie comment **Express** la gère.*

## Bonnes Pratiques

1. **Structure du Projet**

Organisez votre code de manière claire et modulaire.

```
project/
├── routes/          # Routes de l'application
├── controllers/     # Logique métier
├── models/          # Modèles de données
├── middleware/      # Middlewares personnalisés
├── public/          # Fichiers statiques
├── views/           # Templates (si utilisés)
├── config/          # Configuration
└── app.js           # Point d'entrée
```

1. **Configuration**

```jsx
// config.js
const config = {
  port: process.env.PORT || 3000,
  env: process.env.NODE_ENV || 'development',
  db: {
    url: process.env.DB_URL
  }
};

module.exports = config;
```

1. **Asynchronous/Await**

Simplifiez le code asynchrone avec `async` et `await`.

```jsx
app.get('/users', async (req, res, next) => {
  try {
    const users = await User.find();
    res.json(users);
  } catch (error) {
    next(error);
  }
});
```

## Différences Clés avec Node.js Natif

1. **Routing**

Express simplifie grandement la gestion des routes par rapport à Node.js pur.

- Node.js : Gestion manuelle des routes
- Express : Système de routage intégré

```jsx
// Node.js natif
if (req.url === '/api' && req.method === 'GET') {
  // Traiter la requête
}

// Express
app.get('/api', (req, res) => {
  // Traiter la requête
});
```

1. **Middleware**
    - Node.js : À implémenter manuellement
    - Express : Système de middleware intégré
2. **Fichiers Statiques**
    - Node.js : Configuration manuelle
    - Express : `express.static` intégré

## Exemple Complet d'une API REST

```jsx
const express = require('express');
const app = express();

// Middlewares
app.use(express.json());
app.use(express.static('public'));

// Routes
app.get('/api/items', async (req, res) => {
  try {
    const items = await Item.find();
    res.json(items);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.post('/api/items', async (req, res) => {
  try {
    const newItem = await Item.create(req.body);
    res.status(201).json(newItem);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Gestion des erreurs
app.use((err, req, res, next) => {
  console.error(err);
  res.status(500).json({ error: 'Internal Server Error' });
});

// Démarrage
app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```