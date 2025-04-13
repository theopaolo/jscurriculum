# Multer avec Express.js

Quand il s’agit de gérer le téléchargement de fichiers dans une application Node.js, deux options principales s’offrent à vous : utiliser le module natif `fs` (file system) ou recourir à un middleware spécialisé comme **Multer**. Bien que le module `fs` soit puissant pour les opérations de lecture et d’écriture de fichiers, il n’est pas conçu pour traiter les données multipart/form-data provenant des formulaires HTML. C’est ici que Multer entre en jeu.

**Multer** est un middleware Node.js pour Express.js qui facilite la gestion des données `multipart/form-data`, le format standard pour le téléchargement de fichiers depuis un client vers un serveur. Il s’intègre de manière transparente avec Express et offre une multitude de fonctionnalités pour manipuler, valider et stocker les fichiers téléchargés.

## **Pourquoi utiliser Multer plutôt que fs ?**

- **Parsing des données** : Multer gère le parsing des requêtes HTTP contenant des données multipart/form-data, ce que fs ne fait pas nativement.
- **Intégration avec Express** : En tant que middleware, Multer s’intègre facilement dans le pipeline de traitement des requêtes d’Express.
- **Gestion avancée des fichiers** : Multer offre des options avancées pour le stockage, la nomination et la validation des fichiers, simplifiant ainsi le processus de téléchargement.
- **Performance** : Conçu pour être efficace, Multer utilise des streams pour éviter de charger entièrement les fichiers en mémoire.

## Installation et Configuration de Base

### Installation

```bash
npm install multer express
```

### Configuration Basique

Voici comment configurer Multer avec une configuration de base :

```jsx
const express = require('express');
const multer = require('multer');
const path = require('path');

const app = express();

// Configuration de base
const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        cb(null, 'uploads/'); // Dossier de destination
    },
    filename: (req, file, cb) => {
        const uniqueSuffix = Date.now() + '-' + Math.round(Math.random() * 1E9);
        cb(null, `${file.fieldname}-${uniqueSuffix}${path.extname(file.originalname)}`);
    }
});

const upload = multer({ storage: storage });
```

## Options de Configuration Avancées

Multer offre de nombreuses options pour personnaliser la gestion des fichiers.

### Configuration Complète avec Validation

```jsx
const storage = multer.diskStorage({
    destination: (req, file, cb) => {
        // Création du dossier si inexistant
        const uploadDir = 'uploads/';
        if (!fs.existsSync(uploadDir)) {
            fs.mkdirSync(uploadDir, { recursive: true });
        }
        cb(null, uploadDir);
    },
    filename: (req, file, cb) => {
        // Nettoyage du nom de fichier
        const cleanFileName = file.originalname.replace(/[^a-zA-Z0-9]/g, '-');
        const uniqueSuffix = `${Date.now()}-${Math.round(Math.random() * 1E9)}`;
        cb(null, `${cleanFileName}-${uniqueSuffix}${path.extname(file.originalname)}`);
    }
});

// Filtrage des fichiers
const fileFilter = (req, file, cb) => {
    // Liste des types MIME autorisés
    const allowedMimes = [
        'image/jpeg',
        'image/png',
        'image/gif',
        'application/pdf'
    ];

    if (allowedMimes.includes(file.mimetype)) {
        cb(null, true);
    } else {
        cb(new Error('Type de fichier non supporté'), false);
    }
};

// Configuration complète de Multer
const upload = multer({
    storage: storage,
    fileFilter: fileFilter,
    limits: {
        fileSize: 5 * 1024 * 1024, // 5MB
        files: 5 // Maximum 5 fichiers
    }
});
```

## Utilisation dans les Routes

### Upload Simple

```jsx
// Upload d'un seul fichier
app.post('/upload', upload.single('file'), (req, res) => {
    if (!req.file) {
        return res.status(400).send('Aucun fichier uploadé');
    }
    res.json({
        message: 'Fichier uploadé avec succès',
        file: {
            name: req.file.filename,
            path: req.file.path,
            size: req.file.size
        }
    });
});

// Upload multiple
app.post('/upload-multiple', upload.array('files', 5), (req, res) => {
    if (!req.files || req.files.length === 0) {
        return res.status(400).send('Aucun fichier uploadé');
    }
    res.json({
        message: 'Fichiers uploadés avec succès',
        files: req.files.map(file => ({
            name: file.filename,
            path: file.path,
            size: file.size
        }))
    });
});
```

## Utilisation dans un Contrôlleur

Placer la logique de Multer directement dans le contrôleur peut être pratique pour garder tout le code relatif à une route spécifique au même endroit. Voici comment vous pouvez procéder.

```jsx
// controllers/uploadController.js
const multer = require('multer');
const path = require('path');

// Configuration de Multer
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    const uploadDir = 'uploads/';
    cb(null, uploadDir);
  },
  filename: (req, file, cb) => {
    const uniqueName = `${Date.now()}-${file.originalname}`;
    cb(null, uniqueName);
  },
});

const upload = multer({ storage: storage }).single('file');

exports.uploadFile = (req, res) => {
  upload(req, res, (err) => {
    if (err) {
      return res.status(400).json({ error: err.message });
    }
    if (!req.file) {
      return res.status(400).json({ error: 'Aucun fichier fourni' });
    }
    res.json({
      message: 'Fichier uploadé avec succès',
      file: req.file,
    });
  });
};
```

## Upload avec Champs Multiples

```jsx
const fields = [
    { name: 'avatar', maxCount: 1 },
    { name: 'gallery', maxCount: 5 }
];

app.post('/upload-fields', upload.fields(fields), (req, res) => {
    res.json({
        avatar: req.files['avatar'],
        gallery: req.files['gallery']
    });
});
```

## Gestion des Erreurs

```jsx
// Middleware de gestion d'erreurs pour Multer
const multerErrorHandler = (err, req, res, next) => {
    if (err instanceof multer.MulterError) {
        switch (err.code) {
            case 'LIMIT_FILE_SIZE':
                return res.status(400).json({
                    error: 'Fichier trop volumineux',
                    limit: '5MB'
                });
            case 'LIMIT_FILE_COUNT':
                return res.status(400).json({
                    error: 'Trop de fichiers',
                    limit: '5 fichiers maximum'
                });
            case 'LIMIT_UNEXPECTED_FILE':
                return res.status(400).json({
                    error: 'Champ de fichier inattendu'
                });
            default:
                return res.status(400).json({
                    error: 'Erreur lors de l\\'upload',
                    detail: err.message
                });
        }
    }
    next(err);
};

app.use(multerErrorHandler);
```

## Sécurité et Bonnes Pratiques

### Validation des Fichiers

```jsx
const validateFile = (file) => {
    // Vérification du type MIME réel
    const fileType = require('file-type');
    const buffer = readChunk.sync(file.path, 0, fileType.minimumBytes);
    const type = fileType(buffer);

    // Liste des extensions autorisées
    const allowedExtensions = ['.jpg', '.jpeg', '.png', '.pdf'];

    // Vérification de l'extension
    const ext = path.extname(file.originalname).toLowerCase();
    if (!allowedExtensions.includes(ext)) {
        throw new Error('Extension de fichier non autorisée');
    }

    // Vérification de la correspondance entre type MIME déclaré et réel
    if (type && file.mimetype !== type.mime) {
        throw new Error('Type de fichier invalide');
    }
};

```

### Protection Contre les Attaques

```jsx
// Middleware de protection
app.use((req, res, next) => {
    // Limitation de la taille des requêtes
    if (req.headers['content-length'] > 10 * 1024 * 1024) { // 10MB
        return res.status(413).send('Payload trop volumineux');
    }

    // Vérification du type de contenu
    if (req.headers['content-type'] &&
        !req.headers['content-type'].includes('multipart/form-data')) {
        return res.status(415).send('Type de contenu non supporté');
    }

    next();
});
```

## **Stockage sur des Services Cloud**

Vous pouvez intégrer Multer avec des services de stockage cloud comme AWS S3, Google Cloud Storage, etc.

```jsx
// Exemple avec AWS S3
const AWS = require('aws-sdk');
const multerS3 = require('multer-s3');

const s3 = new AWS.S3();

const uploadS3 = multer({
  storage: multerS3({
    s3: s3,
    bucket: 'nom-de-votre-bucket',
    key: (req, file, cb) => {
      cb(null, `uploads/${Date.now().toString()}-${file.originalname}`);
    },
  }),
});
```

*Note : Pour utiliser multer-s3, vous devez l’installer via npm install multer-s3.*

## **Nettoyage des Fichiers Temporaires**

Il est important de nettoyer les fichiers qui ne sont plus nécessaires pour éviter de saturer le stockage.

```jsx
const cleanup = () => {
  const tempDir = 'uploads/';
  fs.readdir(tempDir, (err, files) => {
    if (err) throw err;
    for (const file of files) {
      fs.unlink(path.join(tempDir, file), (err) => {
        if (err) throw err;
      });
    }
  });
};

// Exécuter le nettoyage périodiquement
setInterval(cleanup, 24 * 60 * 60 * 1000); // Toutes les 24 heures
```

## Annexe : Exemple de Frontend

### Formulaire HTML Simple

```html
<form action="/upload" method="post" enctype="multipart/form-data">
    <input type="file" name="file" accept="image/*,.pdf" required>
    <button type="submit">Uploader</button>
</form>
```

### Upload avec JavaScript

```jsx
// Fonction d'upload avec progression
async function uploadFile(file) {
    const formData = new FormData();
    formData.append('file', file);

    try {
        const response = await fetch('/upload', {
            method: 'POST',
            body: formData,
        });

        if (!response.ok) {
            throw new Error(`Erreur HTTP: ${response.status}`);
        }

        const result = await response.json();
        console.log('Upload réussi:', result);
    } catch (error) {
        console.error('Erreur lors de l\\'upload:', error);
    }
}
```

## Points Clés à Retenir

1. **Sécurité**
    - Toujours valider les types de fichiers
    - Limiter la taille des fichiers
    - Générer des noms de fichiers sécurisés
    - Vérifier les types MIME réels
    - Nettoyer les méta-données des fichiers si nécessaire
2. **Performance**
    - Utiliser des limites appropriées
    - Implémenter une gestion de la progression
    - Considérer le stockage en streaming pour les gros fichiers
3. **Maintenance**
    - Nettoyer régulièrement les fichiers temporaires
    - Implémenter une stratégie de backup
    - Logger les opérations d'upload importantes
4. **Tests**
    - Tester tous les cas d'erreur possibles
    - Vérifier la gestion des fichiers corrompus
    - Tester les limites de taille et de nombre