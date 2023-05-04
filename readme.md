# Tuto pour réaliser une application react avec un backend Node et l'uploader sur https://railway.app/
## Préparation du projet
* S'assurer que nodeJS / npm / create-react-app soient installés
* Créer un dossier local qui contiendra notre application (front/back)
* Executer la commande "npm i -y"

## Remplir le package.json que l'on vient de créer

root/package.json
```javascript
{
  "name": "react-node-app",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "start": "node server/app.js",
    "build": "cd client && npm install && npm run build"
  },
  "engines": {
    "node": "14.23"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "dotenv": "^16.0.3",
    "express": "^4.18.2",
    "path": "^0.12.7"
  }
}
```
## Préparation du backend
* Créer un dossier server
* Dans ce dossier créer un fichier app.js et remplissez le comme suis:

```javascript
require('dotenv').config();
const path = require('path');

const PORT = process.env.PORT || 3001;

const app = express();

app.use(express.static(path.resolve(__dirname, '../client/build')));

app.get("/api", (req, res) => {
    res.json({ message: "Hello from server!" });
  });
  app.get('*', (req, res) => {
    res.sendFile(path.resolve(__dirname, '../client/build', 'index.html'));
  });

app.listen(PORT, () => {
  console.log(`Server listening on ${PORT}`);
});
```
*Executez la commande npm i pour installer les packages

## Préparation du frontend
* Executer la commande npx create-react-app@latest client
* Dans le dossier client crée, on rajoute au fichier package.json

```javascript
...
"proxy": "http://localhost:3001",
....
```
## Demander des données au back à partir du front
* Se rendre dans le dossier client
* Modifier le fichier src/App.js

```javascript
    import logo from './logo.svg';
import {useState, useEffect} from 'react'
import './App.css';

function App() {
  const [data, setData] = useState(null);

  useEffect(() => {
    fetch("/api")
      .then((res) => res.json())
      .then((data) => setData(data.message));
  }, []);
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>{!data ? "Loading..." : data}</p>
      </header>
    </div>
  );
}

export default App;
```
## Préparer l'envoi sur github
* Ouvrir un gitbash en administrateur
* Saisir la commande : "rm .git -r" dans le dossier client
* Répondre "y" pour chaque fichier
* Dans le dossier root (contient les dossiers client et server créés avant), éxécuter la commande "git init"
* Executer la commande: git add .
* Executer la commande: git commit -m 'nom arbitraire du commit'
* Executer la commande: git push 

## Sources

* **Reed Barger** - *Initial work* - (https://www.freecodecamp.org/french/news/comment-creer-une-appli-react-avec-un-backend-node-guide-complet/)


