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
* **package.json** contient la liste des *plugins* utilisés dans notre projet. *npm* se base sur le contenu de ce fichier pour télécharger les dépendances.
* **Gruntfile.js** est utilisé pour charger et paramétrer les *plugins*

# Livereload
## CSS
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
Par cette commande, *npm* va aller lire le fichier *package.json* et télécharger les plugins qui correspondant à ceux présents dans *devDependencies**.

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

Ajoutons un fond de couleur rouge à la page en créant le style dans le fichier *main.css* :

```html
body{
    background-color: #FF0000;
}
```

Si on regarde le navigateur il s'est actualisé automatiquement sans que l'on intervienne !

## HTML et JS
Que se passe-t-il si on modifie notre fichier HTML ? Rien ne passe. C'est normal parce qu'on a pas dit à grunt de surveiller les modifications sur les fichiers .html.

Nous allons y remédier. Pour ce faire, nous devons modifier la configuration dans le fichier *Gruntfile.js* plus précisément la partie avec *grunt.initConfig* :
```
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

Et si on veut du livereload pour les fichiers javascripts il faut appliquer le même principe, modifier le *grunt.initConfig*
```
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
            //watch all changes on any html files 
            //and reload the page when they changed
            html: {
                files: '**/*.html',
                options: {
                    livereload: true,
                },
            },
            //watch all changes on any js files 
            //and reload the page when they changed
            js: {
                files: '**/*.js',
                options: {
                    livereload: true,
                },
            }
        }
    });
```
Et voilà maintenant les modifications des fichiers html, css, js entrainent l'actualisation de la page dans le navigateur !

### Optimisation
Si on y regarde de plus près, on remarque qu'on peut optimiser un peu tout ça. Faire un *watch* sur une liste de type de fichier
```
    // Project configuration.
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on any css files 
            //and reload the page when they changed
            front: {
                files: ['**/*.css','**/*.html','**/*.js'],
                options: {
                    livereload: true,
                },
            }
        }
    });
```
On a tout rassembler dans le tableau *files* dans l'objet *front*.

Il se peut que le watch soit lent. Cela peut être dû au fait qu'il regarde trop de fichiers. 

Ce que l'on peut faire c'est soit précisé exactement quelle répertoire regarder (le répertoire css, le répertoire js) ou exclure ce que l'ont pas surveillé comme le répertoire *node_modules* par exemple. 

Pour ce faire on ajoute **'!**/node_modules/**'** à la liste de fichier. Le point d'exclamation au début, signifie exclure
```
    // Project configuration.
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on any css files 
            //and reload the page when they changed
            front: {
                files: ['**/*.css','**/*.html','**/*.js','!**/node_modules/**'],
                options: {
                    livereload: true,
                },
            }
        }
    });
```

# Sass
Si vous utilisez le préprocesseur CSS SASS vous devez être familiarisé avec cette ligne de commandes (plus ou moins)
```
sass --watch scss:css
```
Une fois grunt configuré et lancé, vous n'avez plus besoin de taper cette ligne. 


Mais avant ça nous devons installer le plugin :

```
npm install grunt-contrib-sass --save-dev
```

>Il est important d'ajouter **--save-dev** à la fin de la commande. Si on ne le fais pas, la dépendance ne sera pas ajouté à notre fichier package.json. 
Grunt fonctionnera quand même mais quand vous aller vouloir reprendre je fichier package.json pour pouvoir le réutiliser sur un autre projet, en installant les plugins à l'aide de la commande *npm install*, il n'y aura pas le plugin *grunt-contrib-jshint* !

Une fois le plugin installé, il faut dire à grunt de le charger et le configuré. Nous allons donc modifier le fichier *Gruntfile.js*
```
// Project configuration.
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        //Configuration for watch
        watch: {
            //watch all changes on any css files 
            //and reload the page when they changed
            front: {
                files: ['**/*.css','**/*.html','**/*.js','!**/node_modules/**'],
                options: {
                    livereload: true,
                },
            },
            sass: {
                files: '**/*.scss',
                tasks: ['sass']
            },
        },
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
Nous avons modifié plusieurs choses. Tout d'abord, nous avons ajouté une tâche *sass* qui prends tout le contenu des fichiers .scss qui se trouvent dans le répertoire *scss*, le transforme en css et le met dans le fichier *main.css* qui se trouve dans le répertoire *css* :
```
sass: {
    dev: {
        src: ['scss/*.scss'],
        dest: 'css/main.css',
    },
},
```
Mais pour pouvoir utiliser la tâche sass il faut charger le plugin : 
```
grunt.loadNpmTasks('grunt-contrib-sass');
```
Enfin, on ajoute un *watch* sur les fichiers .scss, pour quand il y'a un changement on appelle la tâche *sass* qui appellera le parser SASS afin de transformer le scss en css.
```
watch: {
    //watch all changes on any css files 
    //and reload the page when they changed
    front: {
        files: ['**/*.css','**/*.html','**/*.js','!**/node_modules/**'],
                options: {
            livereload: true,
                },
    },
    sass: {
        files: '**/*.scss',
        tasks: ['sass']
    },
},
```
