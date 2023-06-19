# De React à Next.js

Je partirai du principe que vous avez déjà des notions de React. 

Lors d'une intégration de React dans une application, que vous avez déjà du voir, voici le type de code que vous avez du écrire :

```jsx
<html>
  <body>
    <div id="app"></div>

    <script src="https://unpkg.com/react@17/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@17/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

    <script type="text/jsx">
      const app = document.getElementById("app")

      function Header({ title }) {
        return <h1>{title ? title : "Default title"}</h1>
      }

      function HomePage() {
        const names = ["Ada Lovelace", "Grace Hopper", "Margaret Hamilton"]

        const [likes, setLikes] = React.useState(0)

        function handleClick() {
          setLikes(likes + 1)
        }

        return (
          <div>
            <Header title="Develop. Preview. Ship. 🚀" />
            <ul>
              {names.map((name) => (
                <li key={name}>{name}</li>
              ))}
            </ul>

            <button onClick={handleClick}>Like ({likes})</button>
          </div>
        )
      }

      ReactDOM.render(<HomePage />, app)
    </script>
  </body>
</html>
```

Vous reconnaîtrez sûrement les éléments suivants :
- La création de composants **Header** et **HomePage** React avec `function`
- La modification d'un état avec `useState`
- Et le rendu de votre app avec `ReactDOM.render`

Voyons maintenant comment vous pouvez passer d'une simple application React à Next.js.

## Premiers pas avec Next.js
---

Pour ajouter **Next.js** à votre projet, vous n'aurez plus besoin de charger les scripts react et react-dom depuis unpkg.com. Au lieu de cela, vous pouvez installer ces packages localement à l'aide du gestionnaire de packages : `npm`.

> Remarque : Vous aurez besoin d'avoir Node.js installé sur votre machine (exigence de version minimale), vous pouvez le télécharger ici.

Pour commencer, créez un nouveau répertoire et initialisez un nouveau projet Node.js à l'intérieur. Vous pouvez le faire en exécutant les commandes suivantes dans votre terminal :

```bash
mkdir nextjs-blog
cd nextjs-blog
npm init -y
```

Ensuite, installez React et React DOM à l'aide de npm :

```bash
npm install react react-dom next
```

Ensuite, ouvrez le fichier `package.json` nouvellement créé et modifiez-le comme suit :

```json
{
  "name": "nextjs-blog",                      // <-- On ajoute ou modifi le nom du projet
  "version": "1.0.0",                         // <-- On ajoute ou modifi une version
  "description": "A sample Next.js project",  // <-- On ajoute une description
  "scripts": {
    "dev": "next dev"                         // <-- On ajoute une commande
  },
  "dependencies": {
    "next": "^10.0.7",
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  }
}
```

Vous remarquerez également un nouveau dossier appelé node_modules qui contient les fichiers réels de nos dépendances

En revenant au fichier index.html, vous pouvez supprimer le code suivant :

- Les scripts `react` et `react-dom` depuis que vous les avez installés avec NPM.
- Les balises `<html>` et `<body>` car Next.js les créera pour vous.
- Le code qui interagit avec l'élément app et la méthode `ReactDom.render()`.
- Le script `Babel` car Next.js a un compilateur qui transforme JSX en JavaScript valide que les navigateurs peuvent comprendre.
- La balise `<script type="text/jsx">`.
- La Réaction. partie de la fonction `React.useState(0)`

Après avoir supprimé les lignes ci-dessus, ajoutez `import { useState } from "react"` en haut de votre fichier. Votre code devrait ressembler à ceci :

```jsx
// index.html
import { useState } from 'react';
function Header({ title }) {
  return <h1>{title ? title : 'Default title'}</h1>;
}

function HomePage() {
  const names = ['Ada Lovelace', 'Grace Hopper', 'Margaret Hamilton'];

  const [likes, setLikes] = useState(0);

  function handleClick() {
    setLikes(likes + 1);
  }

  return (
    <div>
      <Header title="Develop. Preview. Ship. 🚀" />
      <ul>
        {names.map((name) => (
          <li key={name}>{name}</li>
        ))}
      </ul>

      <button onClick={handleClick}>Like ({likes})</button>
    </div>
  );
}
```

Le seul code restant dans le fichier HTML est JSX, vous pouvez donc modifier le type de fichier de `.html` à `.js` ou `.jsx`.

Maintenant, il y a trois autres choses que vous devez faire pour effectuer une transition complète vers une application Next.js :

1. Déplacez le fichier index.js vers un nouveau dossier appelé pages (plus à ce sujet plus tard).
2. Ajoutez l'exportation par défaut à votre composant React principal pour aider Next.js à distinguer le composant à afficher en tant que composant principal de cette page.

```jsx
// ...
  export default function HomePage() {
//  ...
```

3. Ajoutez un script à votre fichier `package.json` pour exécuter le serveur de développement Next.js pendant que vous développez.

```json
// package.json
{
  // ...
  "scripts": {
    "dev": "next dev"
  },
  // ...
}
```

## Exécution du serveur de développement

Pour exécuter le serveur de développement Next.js, exécutez la commande suivante (en fonction de votre gestionnaire de paquets préféré) dans votre terminal :

```bash
npm run dev
```
ou
```bash
yarn dev
```
ou encore
```bash
pnpm dev
```

Pour confirmer que tout fonctionne, vous pouvez afficher votre application en exécutant npm run dev dans votre terminal et en naviguant vers localhost:3000 dans le navigateur. Ensuite, apportez une petite modification au code et enregistrez-le.

Une fois que vous enregistrez le fichier, vous devriez remarquer que le navigateur se met automatiquement à jour pour refléter le changement.

Cette fonctionnalité s'appelle Actualisation rapide. Il vous donne un retour instantané sur toutes les modifications que vous apportez et est préconfiguré avec Next.js.

En surface, il s'agit d'une petite réduction des lignes de code, mais cela aide à mettre en évidence quelque chose : React est une **bibliothèque** qui fournit des **primitives essentielles** pour créer une interface utilisateur interactive moderne. Mais il reste encore du travail à faire pour combiner cette interface utilisateur que vous créez dans une application.

En regardant la migration, vous avez peut-être déjà une idée des avantages de l'utilisation de Next.js. Vous avez supprimé le script babel, un avant-goût de la configuration d'outils complexe à laquelle vous n'avez plus à penser. Vous avez également vu Fast Refresh en action, l'une des nombreuses fonctionnalités d'expérience de développeur auxquelles vous pouvez vous attendre avec Next.js.

## Félicitations pour la création de votre première application Next.js 👏 !

Pour résumer : 
- vous avez exploré les connaissances fondamentales de React et Next.js, 
- vous avez migré d'une simple application React vers une application Next.js.

Vous pouvez maintenant choisir la prochaine étape de votre parcours Next.js. Tu peux:

Dans le chapitre suivante, nous explorerons le fonctionnement de Next.js en introduisant certains concepts de développement Web connexes. Connaître ces concepts vous aidera à élargir votre base et facilitera l'apprentissage de fonctionnalités Next.js plus avancées.
