# Projet Nodejs

# Mini Serveur de Fichiers Statiques

<aside>
<img src="https://www.notion.so/icons/info-alternate_green.svg" alt="https://www.notion.so/icons/info-alternate_green.svg" width="40px" />

Ce projet est proposé afin de combiner les différents modules natifs (`fs`, `path`, et `http`) introduits dans la fiche  [Introduction à Node.js ](https://www.notion.so/Introduction-Node-js-1263324cc3d780019c99c48476827f8b?pvs=21) 

</aside>

## Description du projet

Créez un serveur Node.js simple qui sert des fichiers statiques (HTML, CSS, images, etc.) à partir d'un répertoire spécifique sur votre ordinateur. Les utilisateurs pourront accéder à ces fichiers via leur navigateur web.

## Objectifs d'apprentissage

- Utiliser le module `http` pour créer un serveur web
- Utiliser le module `fs` pour lire des fichiers du système
- Utiliser le module `path` pour manipuler les chemins de fichiers
- Comprendre les concepts de base des requêtes HTTP et des types MIME

## Étapes

1. Créez un nouveau dossier pour votre projet.
2. Créer un dossier `public` dans le projet. C’est ici que nous mettrons nos fichiers statiques à servir (par exemple, un fichier `index.html`, quelques images, d’autres scripts etc.).
3. Créez un fichier `server.js` à la racine de votre projet

**Exemple de fichier `server.js`**

```jsx
const http = require('http');
const fs = require('fs');
const path = require('path');

const PORT = 3000;
const PUBLIC_DIR = path.join(__dirname, 'public');

// Liste restreinte de types MIME autorisés
const MIME_TYPES = {
  '.html': 'text/html',
  '.css': 'text/css',
  '.js': 'text/javascript',
};

const server = http.createServer((req, res) => {
  // Construire le chemin du fichier
  let filePath = path.join(PUBLIC_DIR, req.url === '/' ? 'index.html' : req.url);

  // Obtenir l'extension du fichier
  let extname = path.extname(filePath);

  // Vérifier si le type MIME est autorisé
  let contentType = MIME_TYPES[extname];

  if (!contentType) {
    // Si le type MIME n'est pas autorisé, renvoyer une erreur 415
    res.writeHead(415, { 'Content-Type': 'text/plain' });
    return res.end('415 - Unsupported Media Type');
  }

  // Lire le fichier
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
  console.log(`Serveur en cours d'exécution sur http://localhost:${PORT}/`);
});
```

1. Créez quelques fichiers HTML, CSS, et ajoutez quelques images dans le dossier `public` pour tester le serveur.
2. Lancez le serveur avec la commande `node server.js` et accédez à `http://localhost:3000` dans votre navigateur.

## Défis supplémentaires

Pour rendre le projet plus intéressant, ajoute les suivantes fonctionnalités 

1. Implémentez une page d'erreur 404 personnalisée.
2. Ajoutez un système de journalisation (logging) pour enregistrer les requêtes reçues.
3. Ajouter `.html` par défaut si la requête est faite sans extension