# Méthodes de Tableaux

<aside>
<img src="https://www.notion.so/icons/info-alternate_green.svg" alt="https://www.notion.so/icons/info-alternate_green.svg" width="40px" />

## Définition

Les **tableaux** (*arrays*) sont des **structures de données** incontournable de la programmation, capables de stocker plusieurs valeurs sous un même nom. JavaScript propose une série de **méthodes puissantes** qui permettent de **manipuler**, **filtrer**, et **transformer** les tableaux de manière efficace. La maîtrise des ces méthodes est indispensable pour une manipulation fluide des données, notamment dans les environnements modernes comme **React**.

</aside>

**Les Principales Méthodes de Tableaux**

### 1. **`forEach()`**

La méthode `forEach()` permet d'exécuter une fonction donnée pour chaque élément du tableau. Contrairement à certaines méthodes comme `map()`, elle **ne retourne pas** de nouveau tableau, mais est utile pour appliquer des **effets de bord** (comme afficher les éléments ou modifier l'état).

```jsx
const fruits = ['apple', 'banana', 'mango'];
fruits.forEach((fruit) => console.log(fruit));
// Affiche : apple, banana, mango
```

**Avantages** :

- Simple pour parcourir un tableau.
- Idéal pour les opérations sans retour de valeur (par exemple, log).

### 2. **`map()`**

La méthode `map()` est utilisée pour **transformer un tableau** en un **nouveau tableau** de même longueur, où chaque élément est modifié selon une fonction fournie ***(callback)***.

```jsx
const numbers = [1, 2, 3, 4];
const doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8]
```

**Avantages** :

- **Non destructif** : le tableau d'origine reste intact.
- Utile pour appliquer des transformations aux éléments.

### 3. **`filter()`**

La méthode `filter()` crée un **nouveau tableau** contenant uniquement les éléments qui satisfont une condition.

```jsx
const ages = [12, 18, 25, 30];
const adults = ages.filter(age => age >= 18);
console.log(adults); // [18, 25, 30]
```

**Avantages** :

- Filtrage conditionnel facile à implémenter.
- Utile pour sélectionner des éléments spécifiques dans de grandes collections.

### 4. **`reduce()`**

La méthode `reduce()` permet de **réduire** un tableau à une seule valeur, en appliquant une fonction sur chaque élément (de gauche à droite), tout en accumulant les résultats.

```jsx
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((acc, currentValue) => acc + currentValue, 0);
console.log(sum); // 10
```

**Avantages** :

- Parfait pour des **opérations cumulatives** (sommes, produits, concaténations).
- Utile pour **transformer** un tableau en une autre structure.

### 5. **`find()`**

`find()` retourne le **premier élément** d'un tableau qui satisfait une condition donnée. Si aucun élément n'est trouvé, il retourne `undefined`.

```jsx
const people = [{ name: 'Alice', age: 25 }, { name: 'Bob', age: 17 }];
const adult = people.find(person => person.age >= 18);
console.log(adult); // { name: 'Alice', age: 25 }
```

**Avantages** :

- Rapide pour trouver un élément unique.
- Renvoie **le premier** élément qui correspond.

### 6. **`some()` et `every()`**

- **`some()`** : Vérifie si **au moins un élément** d'un tableau satisfait une condition.
- **`every()`** : Vérifie si **tous les éléments** d'un tableau satisfont une condition.

```jsx
const numbers = [1, 2, 3, 4];
console.log(numbers.some(num => num > 3)); // true
console.log(numbers.every(num => num > 0)); // true
```

**Avantages** :

- **`some()`** : Utile pour vérifier une condition rapidement sans parcourir tout le tableau.
- **`every()`** : Parfait pour les validations globales sur l'ensemble d'un tableau.

### 7. **`sort()`**

La méthode `sort()` trie les éléments d'un tableau **en place** (modifie le tableau existant), en utilisant un tri lexicographique par défaut. Il est possible de fournir une fonction de comparaison pour un tri personnalisé.

```jsx
const scores = [10, 5, 20, 15];
scores.sort((a, b) => a - b);
console.log(scores); // [5, 10, 15, 20]
```

**Avantages** :

- Très flexible avec une fonction de comparaison.
- Tri rapide pour des ensembles d'éléments.

## Exercices Pratiques

### Exercice 1 : Utilisation de `map()` et `filter()`

Créez un tableau contenant les numéros de 1 à 10. Utilisez la méthode `map()` pour créer un nouveau tableau contenant le carré de chaque nombre. Ensuite, utilisez `filter()` pour ne garder que les carrés supérieurs à 20.

### Exercice 2 : Utilisation de `reduce()`

Créez un tableau contenant des prix d'articles. Utilisez `reduce()` pour calculer le total des prix. Ensuite, modifiez la fonction pour ajouter une taxe de 20% à chaque article avant de faire la somme.

### Exercice 3 : Trouver un élément avec `find()`

Créez un tableau d'objets représentant des étudiants avec leurs noms et notes. Utilisez `find()` pour trouver le premier étudiant ayant une note supérieure à 15.

### Exercice 4 : Validation avec `some()` et `every()`

Créez un tableau de numéros représentant des âges. Utilisez `some()` pour vérifier s'il y a au moins un mineur, et `every()` pour vérifier si tous les âges sont supérieurs à 10.

### Exercice 5 : Tri avec `sort()`

Créez un tableau de mots et utilisez `sort()` pour les trier en ordre alphabétique inversé. Ensuite, triez un tableau de nombres en ordre décroissant.

## Conclusion

Les méthodes de tableaux en JavaScript sont des outils **puissants** pour manipuler et transformer des données. En maîtrisant ces méthodes, vous serez capable d'écrire un code plus **concise**, **expressif** et **performant**. Ces compétences sont indispensables pour aborder les frameworks modernes comme **React** où la manipulation d'ensembles de données est fréquente.