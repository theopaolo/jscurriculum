# Challenge  - Application de Notes

<aside>
<img src="https://www.notion.so/icons/info-alternate_green.svg" alt="https://www.notion.so/icons/info-alternate_green.svg" width="40px" />

Dans ce challenge, nous allons plus loin avec dans nodeJS.

C’est une application web simple permettant de gérer des notes personnelles. 

Ce projet vous permet de mettre en pratique vos connaisance de Node.js, d’avoir un  backend simple avec l’utilisation d'une base de données sqlite3.

</aside>

![image.png](Challenge%20-%20Application%20de%20Notes%201273324cc3d780319bbbff42c0a80865/image.png)

## Notions théoriques nécessaires pour faire le projet

<aside>
<img src="https://www.notion.so/icons/brain_green.svg" alt="https://www.notion.so/icons/brain_green.svg" width="40px" />

[Notions théoriques supplémentaires](Challenge%20-%20Application%20de%20Notes%201273324cc3d780319bbbff42c0a80865/Notions%20the%CC%81oriques%20supple%CC%81mentaires%2029fbce7a6a5b4ca7b76a04cb00e4686d.md)

</aside>

## Structure du projet

```
project/
├── public/
│   ├── index.html
│   └── style.css
├── src/
│   ├── server.js
│   ├── database.js
│   ├── router.js
│   └── fileHandler.js
├── database/
│   └── notes.db
└── package.json
```

## Les objectifs

- Créer un serveur HTTP avec Node.js ( vue dans le précédent projet )
- Manipuler une base de données SQLite à l’aide du package `better-sqlite3`
- Gérer des requêtes HTTP (GET, POST, DELETE)
- Servir des fichiers statiques
- Créer une API REST simple
- Manipuler le DOM et faire des requêtes AJAX côté client à l’aide de la `fetch api`

## Fonctionnalités du projet

### Créer un serveur de base

- Créer un serveur HTTP sur le port 3000
- Gérer les routes pour l'API et les fichiers statiques
- Servir les fichiers statiques (HTML, CSS, JS)

### Base de données

- Utiliser SQLite via `better-sqlite3`
- Créer une table 'notes' avec les champs :
    
    ```sql
    - id (INTEGER PRIMARY KEY AUTOINCREMENT)
    - title (TEXT)
    - content (TEXT)
    - created_at (DATETIME)
    ```
    

### API REST

Implémenter les endpoints suivants :

- `GET` **/api/notes** : Récupérer toutes les notes
- `POST` **/api/notes** : Créer une nouvelle note
- `DELETE` **/api/notes/:id** : Supprimer une note

### Interface utilisateur

Créer une interface simple avec :

- Un formulaire pour ajouter des notes
- Une liste des notes existantes
- Un bouton de suppression pour chaque note

# Spécifications techniques

### 1. Méthodes de notre base de données (database.js)

```jsx
// Fonctions à implémenter
initDatabase()    // Initialise la base de données
addNote()         // Ajoute une nouvelle note
getNotes()        // Récupère toutes les notes
deleteNote()      // Supprime une note
```

### 2. Notre  routeur (router.js)

- Gérer les différentes routes de l'application
- Rediriger vers les bons gestionnaires selon l'URL
- Gérer les différentes méthodes HTTP (GET, POST, DELETE)

### 3. Interface web (index.html)

- Formulaire avec champs titre et contenu
- Section d'affichage des notes
- Un minimum de style CSS rendre le projet lisible

# Guide pas à pas

1. Créer la structure de dossiers comme indiquée  au début du challenge
2. Installer les dépendances nécessaires

```bash
npm init -y
npm install better-sqlite3
```

1. Placer les fichiers dans leurs dossiers respectifs selon la structure qui t’as était donnée
2. Créer un dossier `data`  pour y placer ta base de données SQLite

Une fois tout configuré et le code  du pourras lancer le server avec

```bash
node src/server.js
```

---

## 🌟Critères de réussite

1. Le serveur démarre sans erreur
2. Les notes peuvent être ajoutées via le formulaire
3. Les notes s'affichent correctement
4. Les notes peuvent être supprimées
5. Les erreurs sont gérées proprement
6. L'interface est responsive et utilisable 

## 🎁 Bonus si tu arrive à finir rapidement

1. Ajouter la modification des notes
2. Ajouter un système de recherche
3. Ajouter un tri des notes
4. Ajouter une confirmation avant suppression
5. Ajouter un système de catégories