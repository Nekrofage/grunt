# Grunt
Quand on crée des sites internet, il nous arrive de faire souvent les mêmes tâches : 

* *Minifier* la taille des fichiers javascript et css pour gagner en temps de chargement
* Optimiser la taille des images
* Lancer les tests javascript
* ...

Afin de ne pas s'embêter à tout configurer à chaque fois. Des petits malins ont créé les gestionnaires de tâches tels que [grunt](http://gruntjs.com/). 

Nous allons utiliser grunt en ligne de commande. **Toutes les lignes de commandes devront être lancées à partir du répertoire de ce projet.**

# Installation

Pour l'installer il suffit de regarder la documentation officielle [ici](http://gruntjs.com/getting-started).
>Grunt a besoin de npm, le gestionnaire de paquet de node.js pour fonctionner.

## Créer un projet grunt
Grunt a besoin de [2 fichiers](http://gruntjs.com/getting-started#preparing-a-new-grunt-project) pour fonctionner **package.json** et **Gruntfile.js**
* **package.json** contient la liste des *plugins* utilisés dans notre projet. *npm* se base sur ce fichier pour télécharger les fichiers des plugins (les dépendances).
* **Gruntfile.js** est utilisé pour charger et paramétrer les *plugins*

**Attention : ces fichiers doivent se trouver à la racine du répertoire du projet**

# Livereload

Le rafraichissage automatique de la page dans le navigateur web est une fonctionnalité qui peut être très pratique quand on fait de l'intégration. 

En sauvegardant nos modifications la page de rafraîchit automatiquement !

Pour l'utiliser, il faut d'abord installer le plugin *grunt-contrib-watch* mais pas seulement ! **Il faut aussi installer [l'extension](http://livereload.com/extensions/) livereload pour notre navigateur internet**. Pour ma part, je l'ai installé sur Chrome.

> Sous Chrome, pour que l'extension fonctionne il faut activer "Autoriser l'accès aux URL de fichiers" dans les options de l'extension.
Pour ce faire, il faut faire un clique droit sur l'icône de l'extension puis aller sur *Gérer les extensions* Puis cocher la case "Autoriser l'accès aux URL de fichiers" pour le module *LiveReload*.

Une fois l'extension installée, on va installer *grunt-contrib-watch*.

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
Comme dit précédemment, ce fichier contient les dépendances, nos plugins. Pour l'instant *grunt* et *grunt-contrib-watch*. 
Nous allons récupérer les sources des plugins pour les rapatrier sur notre ordinateur :
```console
npm install
```
Par cette commande, *npm* va aller lire le fichier *package.json* et télécharger les plugins correspondant à ceux présents dans **devDependencies**. Si on regarde dans notre dossier, on voit qu'il y a un nouveau répertoire qui s'appelle *node_modules*. C'est là où *npm* a téléchargé les sources des plugins.

Voilà *grunt* et *grunt-watch* sont installés. 

## CSS

Nous allons maintenant configurer grunt pour actualiser automatiquement la page de notre navigateur quand on modifie les fichiers css qui se trouvent dans le répertoire *css*. Nous allons créer le fichier *Gruntfile.js* : 
```javascript
/** Gruntfile.js */
module.exports = function(grunt) {
    // Project configuration.
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on css files in the css directory and subdirectories
            //and reload the page when they changed
            css: {
                files: 'css/**/*.css',
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
Pour tester si ça marche on lance d'abord la commande *grunt* dans le terminal :
```console
grunt
```
Vous devriez avoir quelque chose comme ça :
```console
Running "watch" task
Waiting...
```
Ensuite, on ouvre notre page *index.html* dans notre navigateur et on active la fonctionnalité *livereload* . 

Pour voir si ça marche, ajoutons un fond de couleur rouge à la page en créant un style dans notre fichier *main.css* :

```css
/** css/main.css **/
body{
    background-color: #FF0000;
}
```

Si on regarde le navigateur il s'est actualisé automatiquement sans que l'on intervienne !

## HTML et JS
Que se passe-t-il si on modifie notre fichier HTML ? Rien. C'est normal parce qu'on a pas dit à grunt de surveiller les modifications sur les fichiers html.

Nous allons y remédier. Pour ce faire, nous devons modifier la configuration dans le fichier *Gruntfile.js* plus précisément la partie dans *grunt.initConfig* :
```javascript
grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on css files in the css directory and subdirectories
            //and reload the page when they changed
            css: {
                files: 'css/**/*.css',
                options: {
                    livereload: true,
                },
            },
            //watch all changes on any html files 
            //and reload the page when they changed
            html: {
                files: '**/*.html',
                options: {
                    livereload: true,
                },
            }
        }
    });
```
Pour tester si ça marche il faut arrêter *grunt* et puis le relancer.

Et si on veut du livereload pour les fichiers javascripts qui se trouvent dans le répertoire *js*, il faut appliquer le même principe :
```javascript
grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on css files in the css directory and subdirectories
            //and reload the page when they changed
            css: {
                files: 'css/**/*.css',
                options: {
                    livereload: true,
                },
            },
            //watch all changes on any html files 
            //and reload the page when they changed
            html: {
                files: '**/*.html',
                options: {
                    livereload: true,
                },
            },
            //watch all changes on js files in the js directory and subdirectories
            //and reload the page when they changed
            js: {
                files: 'js/**/*.js',
                options: {
                    livereload: true,
                },
            }
        }
    });
```
Et voilà maintenant les modifications des fichiers html, css, js entrainent l'actualisation automatique de la page !

### Optimisation
Si on y regarde de plus près, on remarque qu'on peut optimiser un peu tout ça. Dans un premier temps, faire un *watch* sur une liste de type de fichier
```javascript
    // Project configuration.
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on html, js and css files 
            //and reload the page when they changed
            front: {
                files: ['css/**/*.css','**/*.html','js/**/*.js'],
                options: {
                    livereload: true,
                },
            }
        }
    });
```
On a tout rassembler dans le tableau *files* et dans l'objet *front*.

Il se peut que le watch soit lent. Cela peut être dû au fait qu'il regarde trop de fichiers. 

On va exclure les répertoires que l'on ne veut pas surveiller car on va jamais rien y modifier. Comme par exemple *node_modules*. 

Pour cela, on ajoute *'!**/node_modules/**'* à la liste de fichier. Le point d'exclamation au début, signifie exclure :
```javascript
    // Project configuration.
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on html, js and css files except files in the directory node_modules
            //and reload the page when they changed
            front: {
                files: ['css/**/*.css','**/*.html','js/**/*.js','!**/node_modules/**'],
                options: {
                    livereload: true,
                },
            }
        }
    });
```

# Sass
Si vous utilisez le préprocesseur SASS vous devez être familiarisé avec cette ligne de commandes (plus ou moins)
```console
sass --watch scss:css
```
Une fois grunt configuré et lancé, vous n'avez plus besoin de taper cette ligne. 


Mais avant ça nous devons installer le plugin :

```console
npm install grunt-contrib-sass --save-dev
```

>Il est important d'ajouter **--save-dev** à la fin de la commande. Si on ne le fais pas, la dépendance ne sera pas ajouté à notre fichier package.json. 
Grunt fonctionnera quand même mais quand vous allez vouloir reprendre je fichier package.json pour pouvoir le réutiliser sur un autre projet, en installant les plugins à l'aide de la commande *npm install*, il n'y aura pas le plugin *grunt-contrib-jshint* !

Une fois le plugin installé, il faut le configurer et dire à grunt de le charger. Nous allons donc modifier le fichier *Gruntfile.js*
```javascript
    // Project configuration.
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on html, js and css files except files in the directory node_modules 
            //and reload the page when they changed
            front: {
                files: ['css/**/*.css','**/*.html','js/**/*.js','!**/node_modules/**'],
                options: {
                    livereload: true,
                },
            },
            //watch all changes on scss files in the scss directory
            //and launch the task sass
            scss: {
                files: 'scss/**/*.scss',
                tasks: ['sass']
            },
        },
        //The sass task
        sass: {
            dev: {
                src: ['scss/*.scss'],
                dest: 'css/main.css',
            },
        },
    });

    // Load the plugin that provides the "watch" task.
    grunt.loadNpmTasks('grunt-contrib-watch');

    // Load the plugin that provides the "sass watch" task.
    grunt.loadNpmTasks('grunt-contrib-sass');


    // Default task(s).
    grunt.registerTask('default',['watch']);
```
Que s'est-il passé ? On va décomposer un peu le code.

Nous avons modifié plusieurs choses. Tout d'abord, nous avons ajouté une tâche *sass* qui prends tout le contenu des fichiers .scss qui se trouvent dans le répertoire *scss*, le transforme en css et le met dans le fichier *main.css* qui se trouve dans le répertoire *css* :
```javascript
sass: {
    dev: {
        src: ['scss/*.scss'],
        dest: 'css/main.css',
    },
},
```
Mais pour pouvoir utiliser la tâche *sass* il faut charger le plugin : 
```javascript
grunt.loadNpmTasks('grunt-contrib-sass');
```
Enfin, on ajoute un *watch* sur les fichiers .scss, pour quand il y'a un changement, on appelle la tâche *scss* qui appellera le parser SASS afin de transformer le scss en css.
```javascript
watch: {
    //watch all changes on any css files 
    //and reload the page when they changed
    front: {
        files: ['css/**/*.css','**/*.html','js/**/*.js','!**/node_modules/**'],
                options: {
            livereload: true,
                },
    },
    scss: {
        files: 'scss/**/*.scss',
        tasks: ['sass']
    },
},
```
# Activité : mise en situation
Vous allez intégrer un formulaire de contact. Celui devra :
* Comporter les champs : Nom, prénom, email, téléphone, sujet, message et le bouton envoyer
* Être responsive
* Être centré horizontalement à la page
* Avoir une validation des champs. C'est à dire, qu'avant la soumission, vous devez vérifier que :
    * Le champ email soit un email valide
    * Le champ téléphone n'accepte que des chiffres uniquement
    * Aucun champ ne peut être vide
* Afficher un message d'erreur quand un champ n'est pas valide désignant le champ et ce que celui-ci attends comme données

Vous pouvez utiliser toutes les librairies que vous voulez. Votre travail sera mis sur github sous le nom de *formulaire-contact*. Ce repo devra comme toujours, comporter un README.md.
## Bonus
Avoir une protection contre les spams.
## Super bonus
Récupérer les informations du formulaire et les envoyer dans votre boîte email. Il faudra faire un peu de PHP et peut-être modifier la configuration de votre *php.ini* :)
# Aller plus loin
Grunt possède plein de [plugins](http://gruntjs.com/plugins). Un qui pourrait être utile est [uglify](https://www.npmjs.com/package/grunt-contrib-uglify). Il permet de réduire la taille des fichiers javascript, ce qui permet à la page de se charge plus vite.

Je vous invite à installer uglify et à le configurer.