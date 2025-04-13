# Gestion des données côté backend

### Trois méthodes différentes pour gérer les données côté backend

1. **Méthode Simple avec express.json()**
    - C'est la méthode recommandée pour la plupart des cas
    - Le middleware `express.json()` parse automatiquement les données JSON
    - Les données sont directement accessibles dans `req.body`
2. **Méthode avec Chunks**
    - Utile pour traiter les données au fur et à mesure de leur réception
    - Les données arrivent en morceaux (chunks)
    - On concatene les chunks dans une variable string
    - À la fin, on parse le JSON
3. **Méthode avec Buffer**
    - Similar à la méthode chunks mais utilise les Buffers
    - Plus efficace pour gérer les données binaires
    - Les chunks sont stockés dans un tableau puis combinés avec `Buffer.concat()`

```jsx
// Frontend (exemple avec fetch)
const addNote = async (noteContent) => {
    try {
        const response = await fetch('http://localhost:3000/api/add-note', {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json'
            },
            body: JSON.stringify({ content: noteContent })
        });
        
        if (!response.ok) {
            throw new Error('Erreur lors de l\'envoi de la note');
        }
        
        const result = await response.json();
        console.log('Note ajoutée avec succès:', result);
    } catch (error) {
        console.error('Erreur:', error);
    }
};

// Backend (Node.js avec Express)
const express = require('express');
const fs = require('fs').promises;
const app = express();

// Middleware pour parser le JSON
app.use(express.json());

// Méthode 1: Utilisation directe de express.json()
app.post('/api/add-note', async (req, res) => {
    try {
        const { content } = req.body;
        await fs.appendFile('notes.md', `\n${content}\n---\n`);
        res.json({ success: true, message: 'Note ajoutée' });
    } catch (error) {
        res.status(500).json({ success: false, error: error.message });
    }
});

// Méthode 2: Gestion manuelle des chunks (si besoin de traitement spécial)
app.post('/api/add-note-raw', (req, res) => {
    let data = '';
    
    req.on('data', chunk => {
        data += chunk;
    });
    
    req.on('end', async () => {
        try {
            const parsedData = JSON.parse(data);
            await fs.appendFile('notes.md', `\n${parsedData.content}\n---\n`);
            res.json({ success: true, message: 'Note ajoutée' });
        } catch (error) {
            res.status(500).json({ success: false, error: error.message });
        }
    });
});

// Méthode 3: Utilisation de Buffer
app.post('/api/add-note-buffer', (req, res) => {
    const chunks = [];
    
    req.on('data', chunk => {
        chunks.push(chunk);
    });
    
    req.on('end', async () => {
        try {
            const data = Buffer.concat(chunks);
            const parsedData = JSON.parse(data.toString());
            await fs.appendFile('notes.md', `\n${parsedData.content}\n---\n`);
            res.json({ success: true, message: 'Note ajoutée' });
        } catch (error) {
            res.status(500).json({ success: false, error: error.message });
        }
    });
});

app.listen(3000, () => {
    console.log('Serveur démarré sur le port 3000');
});
```