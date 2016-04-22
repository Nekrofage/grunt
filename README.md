# Grunt
Quand on créé des sites interne il nous arrive de faire souvent les mêmes tâches : 

* *Minifier* la taille les fichiers javascript et css pour gagner en temps de chargement
* Optimiser la taille des images
* Tester son code javascript
* ...

Afin de ne peux s'embêter à tous configurer tout le temps. Des petits malins ont créés les gestionnaires de tâches. 

Nous allons utiliser grunt en ligne de commande. **Toutes les lignes de commandes devront être lancées à partir du répertoire du projet.**

# Installation

Pour l'installer je vous invite à regarder la documentation officielle [ici](http://gruntjs.com/getting-started).
>Grunt a besoin de npm, le gestionnaire de paquet de node pour fonctionner

## Activité : Créer un projet grunt
On va créer notre premier projet grunt. Grunt a besoin de [2 fichiers](http://gruntjs.com/getting-started#preparing-a-new-grunt-project) pour fonctionner **package.json** et **Gruntfile.js**
* **package.json** contient la liste des *plugins* utilisés dans notre projet
* **Gruntfile.js** est utilisé pour charger et paramétrer les *plugins*

# Livereload
Une fonctionnalité qui peut être pratique quand on fait de l'intégration front-end est le rafraichissage automatique de la page dans le navigateur web. 

Plus la peine de rafraichir la page dans votre navigateur car dès que vous sauvegarder, ça se le fait automatiquement !

Pour l'utiliser, il faut installer le plugin *grunt-contrib-watch* mais pas seulement ! **Il faut aussi installer [l'extension](http://livereload.com/extensions/) livereload pour notre navigateur internet**. Pour ma part, je l'ai installé sur Chrome.

Une fois l'extension installée, on va installer *grunt-contrib-watch*

Tout d'abord, on doit créer le fichier *package.json*
```
{
  "name": "hello-grunt",
  "version": "0.1.0",
  "devDependencies": {
    "grunt": "~0.4.5",
    "grunt-contrib-watch": "^1.0.0"
  }
}
```
Maintenant que j'ai mon fichier qui contient mes dépendances. Il faut aller les installer dans mon répertoire :
```
npm install
```
Voilà *grunt* et *grunt-watch* sont installés. Maintenant il faut les configurer : 
```
module.exports = function(grunt) {
    // Project configuration.
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on any css files 
            //and reload the page when they changed
            css: {
                files: '**/*.css',
                options: {
                    livereload: true,
                },
            },
        }
    });

    // Load the plugin that provides the "watch" task.
    grunt.loadNpmTasks('grunt-contrib-watch');

    // Default task(s).
    grunt.registerTask('default',['watch']);

};
```
Pour tester si ça marche on lance la commande *grunt* dans le terminal
```
grunt
```
Vous devriez avoir quelque chose comme ça :
```
Running "watch" task
Waiting...
```
Ensuite, on ouvre notre page *index.html* dans notre navigateur et on active la fonctionnalité *livereload*. 

À partir de maintenant, à chaque modification du css, la page va se rafraîchir !

# JSHint
À quoi sert *JSHint* ? c'est simple :
>JSHint is a community-driven tool to detect errors and potential problems in JavaScript code and to enforce your team's coding conventions.

En pratique, ça retourne des stats sur votre code js, vous préviens quand une variable est inutilisée, etc.


# Sass
Si vous utilisez le préprocesseur CSS SASS vous devez être familiarisé avec cette ligne de commandes (plus ou moins)
```
sass --watch scss:css
```
Une fois grunt configuré et lancé, vous n'avez plus besoin de taper cette ligne. 