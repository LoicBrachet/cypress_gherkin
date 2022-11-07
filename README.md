# cypress_gherkin

Télécharger et installer node.js à partir du site https://nodejs.org 

Vérifier l'installation en exécutant les commandes suivantes dans le Terminal exécutez :
    node -v puis npm -v

1/ Démarrer un projet vierge 

Avant d'installer Cypress, il faut initialiser npm sur un nouveau projet. Pour cela créez un nouveau dossier, déplacez vous dans ce dossier depuis le Terminal et exécutez la commande suivante:

     npm init -y

Ensuite, toujours dans le même répertoire installez Cypress en exécutant la commande suivante depuis le Terminal :

    npm install cypress --save-dev

Modifier le fichier package.json pour ajouter une nouvelle commande qui facilite l'ouverture de Cypress :

  "scripts": {

    "cy:open": "cypress open"

  }

Ouvrir le Test Runner de Cypress en exécutant la commande suivante :

    npm run cy:open
    
![image](https://user-images.githubusercontent.com/51779120/200290836-4beee100-892e-4b9c-9ef2-3c2f9d379d1f.png)

Quand la boite dde dialogue s'ouvre cliquer sur "E2E Testing"

![image](https://user-images.githubusercontent.com/51779120/200291069-4be42b63-cda5-4c6c-bcb2-ac49e8d1d013.png)

Cliquer sur "Continue"

![image](https://user-images.githubusercontent.com/51779120/200291145-d5d29fd4-b637-4ffd-b653-868edfa75813.png)

Selectionner le navigateur de votre choix

![image](https://user-images.githubusercontent.com/51779120/200291193-2b5051f6-6623-4eb1-b4ba-72242113515a.png)

Cliquer sur "Create a new empty spec"

Votre projet Cypress est configuré

Télécharger l'extension Cucumber

    npm install @badeball/cypress-cucumber-preprocessor

Télécharger l'extension suivante:

    npm install -D @bahmutov/cypress-esbuild-preprocessor

Le projet est configuré ici pour un environnement windows. Pour un environnement Linux, supprimer cucumber-json-formatter.exe

Télécharger le json-formatter correspondant à votre environnement (linux ou autre) à l'adresse suivante:

    https://github.com/cucumber/json-formatter/releases

Placer le fichier chargé à la racine du projet et renommer le comme ceci:

    cucumber-json-formatter.exe

Pour Linux vous trouverez à la racine du projet le fichier cucumber-json-formatter-linux-amd64 que vous renommerez comme indiqué précédemment

Configurer le fichier cypress.config.js comme ceci
   const { defineConfig } = require('cypress')
    const createBundler = require("@bahmutov/cypress-esbuild-preprocessor");
    const createEsbuildPlugin = require('@badeball/cypress-cucumber-preprocessor/esbuild').createEsbuildPlugin;
    const addCucumberPreprocessorPlugin = require('@badeball/cypress-cucumber-preprocessor').addCucumberPreprocessorPlugin;
    
    async function setupNodeEvents(on, config) {
    await addCucumberPreprocessorPlugin(on, config);
        
    on(
        "file:preprocessor",
            createBundler({
            plugins: [createEsbuildPlugin(config)],
            })
        
    );
    }
    
    module.exports = defineConfig({
    e2e: {
        specPattern: "**/*.feature",
        excludeSpecPattern: [
        "*.js",
        "*.md"
        ],
        chromeWebSecurity: false,
        supportFile: false,
        setupNodeEvents
    }
    
    })

Ajouter les lignes suivantes dans la rubrique dependencies du package.json

     "cypress-cucumber-preprocessor": {
    "json": {
      "enabled": true,
      "formatter": "cucumber-json-formatter",
      "output": "cucumber-report.json"
    }
}

2/Démarrer un projet depuis ce repo git:

cloner le projet

    git clone https://github.com/LoicBrachet/cypress_gherkin.git

à la racine du projet taper la commande suivante pour installer l'ensemble du projet

    npm i

3/Exécution des tests

lancer les tests et générer le rapport sous format json en tapant la commande suivante:

    npx cypress run --spec 'cypress/e2e/**/*.feature'
    
lancer simplement les tests

    npm run cy:open

