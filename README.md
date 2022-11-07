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

Télécharger l'extension Cucumber
    npm install @badeball/cypress-cucumber-preprocessor

Télécharger l'extension suivante:
    npm install -D @bahmutov/cypress-esbuild-preprocessor

Configurer le fichier cypress.config.js comme ceci
    import { defineConfig } from "cypress";
    import createBundler from "@bahmutov/cypress-esbuild-preprocessor";
    import { addCucumberPreprocessorPlugin } from "@badeball/cypress-cucumber-preprocessor";
    import createEsbuildPlugin from "@badeball/cypress-cucumber-preprocessor/esbuild";

    export default defineConfig({
    e2e: {
        specPattern: "**/*.feature",
        async setupNodeEvents(
        on: Cypress.PluginEvents,
        config: Cypress.PluginConfigOptions
        ): Promise<Cypress.PluginConfigOptions> {
        // This is required for the preprocessor to be able to generate JSON reports after each run, and more,
        await addCucumberPreprocessorPlugin(on, config);

        on(
            "file:preprocessor",
            createBundler({
            plugins: [createEsbuildPlugin(config)],
            })
        );

        // Make sure to return the config object as it might have been modified by the plugin.
        return config;
        },
    },
    });

Télécharger les dépendances suivantes pour générer des rapports en json:
    npm install --save-dev mochawesome mochawesome-merge mochawesome-report-generator

Rajouter les lignes suivantes dans le config.js
    reporter: 'mochawesome',
    reporterOptions: {
    reportDir: 'cypress/results',
    overwrite: false,
    html: false,
    json: true
  }