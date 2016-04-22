# Grunt
Quand on créé des sites interne il nous arrive de faire souvent les mêmes tâches : 

* *Minifier* la taille les fichiers javascript et css pour gagner en temps de chargement
* Optimiser la taille des images
* Tester son code javascript
* ...

Afin de ne peux s'embêter à tous configurer tout le temps. Des petits malins ont créés les gestionnaires de tâches. 

# Installation

Pour l'installer je vous invite à regarder la documentation officielle [ici](http://gruntjs.com/getting-started).
>Grunt a besoin de npm, le gestionnaire de paquet de node pour fonctionner

## Activité 
Créer un dossier se nommant *hello-grunt*. Ce dossier contiendra les fichiers de bases pour créer une page html.

Dans ce dossier, créez les fichiers et les répertoires comme ci-dessous.

```
|-- index.html
|-- css
    |-- main.css
|-- js
    |-- main.js
```

Le fichier *index.html* contient le code de base pour créer une page HTML 


# JSHint
Qu'est ce que c'est ? c'est simple :
>JSHint is a community-driven tool to detect errors and potential problems in JavaScript code and to enforce your team's coding conventions.

En pratique, ça retourne des stats sur votre code js, vous préviens quand une variable est inutilisée, etc.

# Livereload
Une fonctionnalité qui peut être pratique quand on fait de l'intégration front-end est le rafraichissage automatique de la page dans le navigateur web. 

Plus la peine de rafraichir la page dans votre navigateur car dès que vous sauvegarder, ça se le fait automatiquement !

# Sass
Si vous utilisez le préprocesseur CSS SASS vous devez être familiarisé avec cette ligne de commandes (plus ou moins)
```
sass --watch scss:css
```
Une fois grunt configuré et lancé, vous n'avez plus besoin de taper cette ligne. 