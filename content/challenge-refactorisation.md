# Challenge - Refactorisation  en Express.js

## Notre projet précédent

[Archive.zip](/content/challenge-refactorisation/Archive.zip)

## Structure du Projet Refactorisé

La structure suivante permet d’organiser votre projet de manière modulaire et maintenable :

```
project/
├── src/
│   ├── app.js              # Fichier principal de configuration Express
│   ├── routes/
│   │   └── notes.js       # Définit les routes associées à la gestion des notes
│   ├── controllers/
│   │   └── notesController.js  # Contient la logique métier de l’application
│   ├── models/
│   │   └── Note.js         # Définition du modèle de données pour les notes
│   ├── middleware/
│   │   ├── errorHandler.js # Gère les erreurs globales de l’application
│   │   └── logger.js       # Middleware pour journaliser les requêtes
│   └── config/
│       └── database.js     # Configuration de la base de données (SQLite ici)
├── public/
│   ├── index.html          # Page d'accueil statique
│   ├── css/
│   │   └── style.css       # Feuille de styles CSS
│   └── js/
│       └── main.js         # Scripts JavaScript pour la partie frontend
├── tests/                  # Tests unitaires et d'intégration
│   └── notes.test.js       # Test pour les fonctionnalités de gestion des notes
└── package.json            # Fichier de gestion des dépendances et scripts
```

**Chaque dossier a un rôle spécifique :**

- **src/** : Contient le code source de l’application.
- **public/** : Sert les fichiers statiques accessibles au navigateur (HTML, CSS, JS).
- **tests/** : Contient les tests de l’application pour valider les fonctionnalités.

## Exemple de re-factorisation

Dans notre premier challenge nous avions un fichier `server.js`  comme ceci

```jsx
const http = require("http");
const path = require("path");
const fs = require('fs');

const PORT = 3000;
const PUBLIC_DIR = path.join(__dirname, '../public');

const MIME_TYPES = {
    '.html': 'text/html',
    '.css': 'text/css',
    '.js': 'text/javascript',
  };

const server = http.createServer((req, res) => {
    let filePath = path.join(PUBLIC_DIR, req.url === '/' ? 'index.html' : req.url);
    let extname = path.extname(filePath);
    let contentType = MIME_TYPES[extname];

    if (!contentType) {
        // Si le type MIME n'est pas autorisé, renvoyer une erreur 415
        res.writeHead(415, { 'Content-Type': 'text/plain' });
        return res.end('415 - Unsupported Media Type');
    }

    fs.readFile(filePath, (err, content) => {
        if (err) {
            if (err.code === 'ENOENT') { //“Error NO ENTry”
              // Fichier non trouvé
              fs.readFile(path.join(PUBLIC_DIR, '404.html'), (err, content) => {
                res.writeHead(404, { 'Content-Type': 'text/html' });
                res.end(content, 'utf-8');
              });
            } else {
              // Erreur serveur
              res.writeHead(500);
              res.end(`Erreur serveur: ${err.code}`);
            }
          } else {
            // Succès
            res.writeHead(200, { 'Content-Type': contentType });
            res.end(content, 'utf-8');
          }
    });
});

server.listen(PORT, () => {
    console.log(`Notre serveur est accessible sur : http://localhost:${PORT}/`);
});
```

## **Nouvelle version avec Express.js**

Avec Express, le code devient plus lisible, modulaire, et maintenable. Voici comment refactoriser ce serveur basique avec Express :

```jsx
const express = require('express');
const path = require('path');

const app = express();
const PORT = 3000;

// Servir les fichiers statiques depuis le dossier 'public'
app.use(express.static(path.join(__dirname, '../public')));

// Gestion des 404 - doit être après les routes et les fichiers statiques
app.use((req, res) => {
    res.status(404).sendFile(path.join(__dirname, '../public/404.html'), err => {
        if (err) res.status(404).send('Page not found');
    });
});

// Démarrage du serveur
app.listen(PORT, () => {
    console.log(`Server running at http://localhost:${PORT}/`);
});
```

**C'est tout 🪄 ! Express fait tout le travail lourd pour nous :**

- Gère automatiquement les types MIME
- Sert les fichiers statiques efficacement
- S'occupe des en-têtes HTTP
- Gère la compression des fichiers

Cette version est :

1. Plus maintenable
2. Plus performante (Express optimise la livraison des fichiers statiques)
3. Plus facile à étendre si besoin
4. Suit les meilleures pratiques d'Express

## Notre app.js Express

```jsx
const express = require('express');
const path = require('path');
const notesRoutes = require('./routes/notes');
const errorHandler = require('./middleware/errorHandler');
const logger = require('./middleware/logger');

const app = express();

// Middleware
app.use(express.json());
app.use(express.static(path.join(__dirname, '../public')));
app.use(logger);

// Routes
app.use('/api/notes', notesRoutes);

// Error handling
app.use(errorHandler);

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server running on port ${PORT} http://localhost:${PORT}`);
});

module.exports = app;
```

## Système de Routage Express

```jsx
// src/routes/notes.js
const express = require('express');
const router = express.Router();

const {
    getAllNotes,
    createNote,
    deleteNote
} = require('../controllers/notesController');

router.get('/', getAllNotes);
router.post('/', createNote);
router.delete('/:id', deleteNote);

module.exports = router;
```

## Contrôleurs

Les contrôleurs gèrent la logique métier de l’application :

```jsx
// src/controllers/notesController.js
const Note = require('../models/Note');

exports.getAllNotes = async (req, res, next) => {
    try {
        const notes = await Note.getAll();
        res.json(notes);
    } catch (error) {
        next(error);
    }
};

exports.getNote = async (req, res, next) => {
    // A toi de jouer
};

exports.createNote = async (req, res, next) => {
    try {
        const { title, content } = req.body;
        const note = await Note.create({ title, content });
        res.status(201).json(note);
    } catch (error) {
        next(error);
    }
};

exports.updateNote = async (req, res, next) => {
    // A toi de jouer
};

exports.deleteNote = async (req, res, next) => {
    // A toi de jouer
};
```

## Middleware

Les middleware sont des fonctions intermédiaires qui enrichissent le processus de requête/réponse. Voici deux exemples utiles dans notre projet :

```jsx
// src/middleware/logger.js
const logger = (req, res, next) => {
    console.log(`${req.method} ${req.url}`);
    next();
};

// src/middleware/errorHandler.js
const errorHandler = (err, req, res, next) => {
    console.error(err.stack);
    res.status(err.status || 500).json({
        error: {
            message: err.message,
            status: err.status
        }
    });
};
```

## Config pour notre db

```jsx
// src/config/database.js
const Database = require('better-sqlite3');
const path = require('path');

const db = new Database(path.join(__dirname, '../../database/notes.db'));

// Initialisation de la table
db.exec(`
    CREATE TABLE IF NOT EXISTS notes (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        title TEXT NOT NULL,
        content TEXT NOT NULL,
        created_at DATETIME DEFAULT CURRENT_TIMESTAMP
    )
`);

module.exports = db;
```

## Modèles avec Better-SQLite3

Le modèle Note.js encapsule les interactions avec la base de données :

```jsx
// src/models/Note.js
const db = require('../config/database');

class Note {
    static getAll() {
        const stmt = db.prepare('SELECT * FROM notes ORDER BY created_at DESC');
        return stmt.all();
    }

    static getById(id) {
        const stmt = db.prepare('SELECT * FROM notes WHERE id = ?');
        return stmt.get(id);
    }

    static create(noteData) {
        const { title, content } = noteData;
        const stmt = db.prepare('INSERT INTO notes (title, content) VALUES (?, ?)');
        const result = stmt.run(title, content);
        return { id: result.lastInsertRowid, title, content };
    }

    static update(id, noteData) {
        const { title, content } = noteData;
        const stmt = db.prepare('UPDATE notes SET title = ?, content = ? WHERE id = ?');
        return stmt.run(title, content, id);
    }

    static delete(id) {
        const stmt = db.prepare('DELETE FROM notes WHERE id = ?');
        return stmt.run(id);
    }
}

module.exports = Note;
```

## **Tests et Validation**

```bash
// tests/notes.test.js
const request = require('supertest');
const app = require('../src/app');

describe('API Notes', () => {
    it('devrait créer une nouvelle note', async () => {
        const res = await request(app)
            .post('/api/notes')
            .send({ title: 'Nouvelle Note', content: 'Contenu' });

        expect(res.status).toBe(201);
        expect(res.body).toHaveProperty('id');
    });
});
```

## Points Clés de la Migration

### Le projet refactorisé

[Archive.zip](Challenge%20-%20Refactorisation%20en%20Express%20js%201293324cc3d7801dbe86d09e275532c0/Archive%201.zip)

### 1 Principaux Changements

1. **Routage**
    - Passage d'un routage manuel à Express Router
    - Organisation modulaire des routes
2. **Middleware**
    - Utilisation de middleware intégrés (express.json())
    - Création de middleware personnalisés
3. **Gestion des Erreurs**
    - Utilisation du middleware de gestion d'erreurs Express
    - Centralisation de la gestion des erreurs

### 2 Avantages de la Migration

- Code plus maintenable et organisé
- Meilleure gestion des erreurs
- Middleware réutilisables
- Routes plus lisibles et maintenues
- Tests plus faciles à écrire

## Améliorer le projet

1. Implémenter la modification des notes (PUT /api/notes/:id)
2. Ajouter une validation des données avec express-validator
3. Implémenter une pagination des notes
4. Ajouter une recherche de notes
5. Implémenter un système de catégories pour les notes

## Notion importante à retenir

- Expliquer le concept de middleware Express
- Démontrer la gestion des erreurs async/await
- Présenter les bonnes pratiques de structure de projet
- Montrer l'importance des tests
- Expliquer la différence entre Node.js natif et Express.js