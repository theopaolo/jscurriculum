# Introduction à Node.js

## Qu’est-ce que Node.js ?

<aside>
**Node.js** est un environnement d’exécution JavaScript côté serveur. Il permet d’exécuter du JavaScript en dehors d’un navigateur web, notamment sur un serveur. Il est construit sur le moteur JavaScript **V8** de Google Chrome, qui compile le code JavaScript en code machine, rendant son exécution extrêmement rapide.
</aside>

## Pourquoi utiliser Node.js ?

Node.js est particulièrement adapté aux applications qui nécessitent des performances élevées, une gestion efficace des I/O, et une grande **scalabilité**. Il est couramment utilisé pour des applications telles que :

- Les serveurs web haute performance
- Les APIs rapides et non-bloquantes
- Les applications en temps réel comme les chats ou les jeux multijoueurs

## **Caractéristiques principales :**

### **Basé sur des événements (Event-driven)**

Node.js repose sur un modèle orienté événements. Cela signifie qu’il fonctionne en réponse à des événements (comme des requêtes HTTP, des messages sur un socket, ou des opérations de fichiers). Cela permet de développer des systèmes réactifs qui réagissent immédiatement à des événements sans consommer inutilement de ressources.

### **Non-bloquant (Asynchrone)**

Node.js est asynchrone par nature. Les opérations d’entrée/sortie (I/O) comme la lecture/écriture de fichiers, la gestion de requêtes réseau, ou les requêtes vers une base de données, sont exécutées en arrière-plan. Cela permet à l’application de continuer à traiter d’autres requêtes sans attendre que l’opération se termine, optimisant ainsi les performances.

### **Single-threaded mais scalable**

Bien que Node.js fonctionne sur un seul thread (monothread), il est capable de gérer des milliers de connexions simultanées grâce à son **Event Loop** et à sa capacité à déléguer les opérations lourdes à des threads en arrière-plan (grâce à la bibliothèque **libuv**). Cela rend Node.js idéal pour des applications nécessitant une forte scalabilité comme les applications temps réel (ex : chat en direct).

```jsx
console.log("Bienvenue dans Node.js!");
```

## **Fonctionnement interne :**

### **Boucle d’événements (Event Loop)**

La **boucle d’événements** est au cœur de Node.js. Elle permet à Node.js de gérer efficacement les opérations asynchrones. Elle écoute les événements et exécute le code correspondant à ces événements dès qu’ils se produisent, tout en continuant à accepter d’autres événements sans bloquer.

### **libuv**

Node.js utilise la bibliothèque **libuv** pour gérer les opérations I/O (réseau, système de fichiers, etc.). Cette bibliothèque est responsable de l’exécution des opérations lourdes en arrière-plan, permettant à Node.js de rester non bloquant tout en étant capable d’effectuer des tâches nécessitant beaucoup de temps (comme des opérations réseau ou de lecture/écriture).

<aside>


## Les différents moteurs javascript

### Dans nos navigateurs web

**Chrome** **:** et les variantes chormium utilisent le moteur v8

**Safari** **:** JavaScriptCore (également appelé **Nitro**)

**Firefox :** SpiderMonkey

### Dans les environements côté serveur

NodeJS et Deno : utilsent le moteur **V8**

Bunjs : utilise le moteur **JavaScriptCore (Nitro)** d’appel pour ces performance amélioré

</aside>

## **Modules natifs en Node.js**

Node.js inclus avec une série de **modules natifs** (également appelés **modules built-in**) qui facilitent les interactions avec le système, les fichiers, le réseau, etc. Voici quelques-uns des modules les plus couramment utilisés.

A la suite un selection des modules : `fs` , `path` et `http` , nodejs propose plusieurs autres modules natifs en plus de l’immense ecosystem de module NPM, qui permetron d’ajouter une infinité de possibilité à nos applications node.

### **Module `fs` (file system)** :

Le module fs permet d’interagir avec le **système de fichiers**.
Il propose des méthodes pour **lire**, **écrire**, **supprimer** et **manipuler** des fichiers, soit de manière asynchrone (non-bloquante), soit de manière synchrone.

```jsx
const fs = require('fs');

// Lecture du fichier en mode asynchrone (avec callbacks)
fs.readFile('hello.txt', 'utf8', (err, data) => {
  if (err) {
    throw err;
  }
  console.log(data);
});

// Utilisation des promesses pour une lecture asynchrone
const fsPromises = require('fs').promises;

async function readFileText(filePath) {
  try {
    const data = await fsPromises.readFile(filePath, 'utf8');
    console.log(data);  // data est déjà en format string grâce à 'utf8'
  } catch (error) {
    console.error('Error reading file:', error.message);
  }
}

// Appel de la fonction asynchrone
readFileText('hello.txt');
```

### **Module `path`**

Le module path permet de manipuler des chemins de fichiers et répertoires de manière indépendante du système d’exploitation (Windows, Linux, macOS).

**Ce que permet le module path :**

1. **Manipulation des chemins** : Créer et gérer les chemins de manière flexible.
2. **Indépendance du système** : Résoudre les différences de séparateurs de chemins (”/” sous Unix/Linux et “” sous Windows).
3. **Gestion des chemins relatifs et absolus** : Résoudre des chemins relatifs en chemins absolus.
4. **Extraction d’informations** : Extraire le répertoire, le nom de fichier, ou l’extension d’un chemin donné.

La méthode path.join() combine plusieurs segments de chemins en un seul chemin, en insérant automatiquement le bon séparateur (slash) selon le système d’exploitation.

```jsx
const path = require('path');

// Construction d'un chemin de fichier
const fullPath = path.join('/home', 'user', 'docs', 'file.txt');

// Affichage du chemin complet
console.log(fullPath);
// Résultat : '/home/user/docs/file.txt' sur Unix | '\home\user\docs\file.txt' sur Windows
```

### **Module `http`** : Permet de créer des serveurs HTTP.

```jsx
const http = require("http");
const server = http.createServer((req, res) => {
  res.writeHead(200, { "Content-Type": "text/plain" });
  res.end("Hello World\n");
});
server.listen(3000, () => {
  console.log("Server running at http://localhost:3000/");
});
```

**Le module HTTP peux aussi être utiliser pour réaliser des requêtes HTTP avec https.get()**

La méthode https.get() permet de faire des requêtes HTTP pour récupérer des données depuis une API. Cela est particulièrement utile pour les applications qui ont besoin de consommer des services externes comme des APIs REST.

**Exemple : Requête vers une API avec https.get()**

```jsx
const https = require("https");
let request = https.get(
  "https://api.capy.lol/v1/capybaras?random=true?json=true",
  (res) => {
    if (res.statusCode !== 200) {
      console.error(
        `Did not get an OK from the server. Code: ${res.statusCode}`
      );
      res.resume();
      return;
    }
    let data = "";
    res.on("data", (chunk) => {
      data += chunk;
    });
    res.on("close", () => {
      console.log("Retrieved all data");
      console.log(JSON.parse(data));
    });
  }
);
request.on("error", (err) => {
  console.error(
    `Encountered an error trying to make a request: ${err.message}`
  );
});
```

**Requêtes avec https.request() pour plus de contrôle**
La méthode https.request() permet de personnaliser davantage les requêtes HTTP, en définissant des options comme la méthode (GET, POST), les en-têtes, et autres paramètres.

```jsx
const https = require('https');

const options = {
  host: 'api.capy.lol',
  path: '/v1/capybaras?random=true?json=true',
  method: 'GET',
  headers: {
    'Accept': 'application/json'
  }
};

// Faire la requête HTTPS
let request = https.request(options, (res) => {
  if (res.statusCode !== 200) {
    console.error(`Did not get an OK from the server. Code: ${res.statusCode}`);
    res.resume();
    return;
  }

  let data = '';

  // Récupérer les données par morceaux (chunks)
  res.on('data', (chunk) => {
    data += chunk;
  });

  // Lorsque toute la réponse est reçue
  res.on('close', () => {
    console.log('Données récupérées');
    console.log(JSON.parse(data));
  });
});

// Fin de la requête
request.end();

// Gestion des erreurs de la requête
request.on('error', (err) => {
  console.error(`Erreur lors de la requête : ${err.message}`);
});
```

## Modules

En Node.js, un module est un fichier contenant du code réutilisable. Le système de modules permet d'organiser le code en unités logiques et de gérer les dépendances entre différentes parties de votre application.

### **Méthodes d'Exportation**

Il existe plusieurs façons d'exporter des fonctionnalités depuis un module :

1. **module.exports** - Pour exporter un objet unique
2. **exports** - Pour exporter plusieurs éléments individuellement
3. **class** - Pour exporter une classe

## NPM (Node Package Manager)

NPM est le gestionnaire de paquets par défaut pour Node.js. Il nous permettra  d’installer, partager et gérer les dépendances de leurs projets.

### Fonctionnalités clés de NPM :

- Installation de packages : `npm install <nom-du-package>`
- Gestion des dépendances via le fichier `package.json`
- Exécution de scripts définis dans `package.json`
- Publication de packages sur le registre NPM

### Exemple d’utilisation :

```jsx
// Installation d'un package
npm install express

// Utilisation dans le code
const express = require('express');
const app = express();
```

## Frameworks populaires pour Node.js

Node.js dispose d’un riche écosystème de frameworks qui simplifient le développement d’applications web :

**Express.js** : Framework web minimaliste et flexible

**Nest.js** : Framework pour construire des applications serveur efficaces et évolutives

**Koa.js** : Framework web moderne et léger par les créateurs d’Express

**Sails.js** : Framework MVC pour la construction rapide d’applications web

---

## **Conclusion**

**Node.js** est un environnement d’exécution puissant, idéal pour les applications web modernes nécessitant une gestion efficace des entrées/sorties, telles que les **APIs**, les **serveurs web** ou les **applications temps réel**. Grâce à ses fonctionnalités non-bloquantes et à ses modules natifs, Node.js permet aux développeurs de créer des systèmes évolutifs, rapides et performants. La maîtrise des **modules natifs**, de **NPM**, et de l’**Event Loop** est essentielle pour exploiter pleinement le potentiel de Node.js.

---

## Exercices pratique

### 1. Lecture de fichier avec `fs`

Créez un script Node.js qui lit le contenu d’un fichier texte et l’affiche dans la console. Utilisez le module `fs` pour cette opération.

### 2. Création d'un module personnalisé

Étendez l'exercice 1 en créant un module personnalisé `fileReader.js` qui exporte une fonction pour lire des fichiers.

*Utiliser ce module dans votre script principal.*

### 3. Création d’un serveur HTTP

Créez une script Node.js qui utilise le module `http` pour servir le contenu du fichier lu dans les exercices précédents. L’application doit écouter sur le port 3000 et afficher le contenu du fichier lorsqu’on accède à [http://localhost:3000](http://localhost:3000/).

### 4. Récupération de données depuis une API et écriture dans un fichier JSON

**Exemple d’API à utiliser :** `https://api.capy.lol/v1/capybaras?random=true&json=true.`

1. Utilisez le module https pour faire une requête vers une API publique et récupérer des données.
2. Une fois les données reçues, utilisez le module `fs` pour écrire ces données dans un fichier JSON.

---

### **Pour notre ami Ethan, voici quelques applications potentielles de cet exercice  :**

- Récupération automatique de données : Récupérer des informations externes régulièrement pour les stocker dans une base de données ou pour les analyser plus tard.
- Systèmes d’agrégation de données : Créer des systèmes qui combinent des informations provenant de plusieurs sources API.
- Échanges de données entre systèmes : Enregistrer des réponses d’API dans des fichiers pour les traiter ou les afficher dans des tableaux de bord.

---

<aside>


Une fois que tu as finaliser les exercises tu pourrais t’attaquer à ce petite projet :

[Projet Nodejs](Introduction%20a%CC%80%20Node%20js%201273324cc3d780dabb16cfa73cf35df6/Projet%20Nodejs%201273324cc3d780b78e1ef6dd1a2f76ea.md)

</aside>