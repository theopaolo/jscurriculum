# Closures dans les framework front

## React

Les closures sont utilisées dans React pour capturer les variables d’état à un moment donné du cycle de vie d’un composant. Cela se produit souvent lorsque des fonctions comme setState sont appelées dans des callbacks. Sans closure, il serait difficile de maintenir une relation entre l’état du composant et les événements ou effets asynchrones.
Exemple d’utilisation de closures dans un useEffect :

```jsx
function MyComponent() {
    const [count, setCount] = useState(0);

    useEffect(() => {
        const handle = setInterval(() => {
            setCount((prevCount) => prevCount + 1); // Closure sur `prevCount`
        }, 1000);

        return () => clearInterval(handle);
    }, []);

    return <div>{count}</div>;
}
```

Ici, la closure permet à setCount de se rappeler de la valeur actuelle de prevCount même dans le contexte d’une fonction asynchrone (comme le setInterval).

## Vue

Gestion des événements dans Vue.js : Vue utilise également des closures pour capturer l’état réactif des composants dans ses directives et méthodes. Lorsqu’une méthode est appelée en réponse à un événement, elle peut capturer les valeurs de l’état local du composant grâce à la fermeture.

```jsx
export default {
    data() {
        return {
            count: 0
        };
    },
    methods: {
        increment() {
            this.count++; // `this.count` est capturé grâce à la closure
        }
    }
};
```

## Conclusion

Les closures jouent un rôle crucial dans JavaScript pour la gestion de l’état, les variables privées, les callbacks et la gestion des événements. Elles sont largement utilisées dans les frameworks front-end comme React, Vue, ou Svelte pour capturer l’état des composants et assurer la réactivité du DOM en réponse à des changements de données ou d’événements.