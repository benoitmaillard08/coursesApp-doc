==================
AngularJS
==================

#############
Introduction
#############

AngularJS est un framework Javascript crée et maintenu depuis 2009 par Google, l'entreprise a l'origine du populaire moteur de recherche, du client Gmail mais aussi dans notre cas de beaucoup d'outils pour les développeurs. En effet outre le fait de créer des outils de programmation comme AngularJS, Google propose des outils d'analyse (`Google Analytics <http://google.com/analytics>`_), des outils de stockage (`Google Cloud <https://cloud.google.com>`_) ou encore des serveurs pout hébérger des applications. Une entreprise non seulement très présente dans le domaine public mais également dans de le soutien et la recherche dans les technologies informatiques. Pour revenir à ce qui nous intéresse, ils ont donc développé le framework AngularJS. Un framework est un ensemble de structures et d'outils programmés dans un language et reutilisables permettant de faciliter la programmation d'applications. On trouve par exemple chez PHP le framework `Symfony <http://symfony.com>`_, chez Ruby `Ruby on Rails <http://rubyonrails.com>`_, chez Python évidemment Django que nous utilisons pour notre site web et finalement chez Javascript AngularJS, mais encore comme concurrent `Ember <http://emberjs.com>`_ ou `Backbone <http://backbonejs.org>`_.

AngularJS permet de développer une application web complète. C'est-à-dire qu'il offre plusieurs outils indispensables. D'abord un système de routes qui permet de lier des URLs avec des pages différentes, en clair on peut dire à notre application que quand l'utilisateur entre *monsite.com/contact* dans son navigateur, il aille chercher et qu'il montre le fichier contact.html. Aussi la prise en charge de formulaire avec la possibilité de récupérer les informations entrées par l'utilisateur ou de faire une validation du formulaire, de vérifier les informations entrées. AngularJS ne sait pas comment communiquer directement avec une base de données - une base de données est l'endroit où sont stockées les données persistentes sous forme de tableaux comme par exemple les comptes utilisateurs d'un site -, par contre il est tout à fait capable d'utliser les données d'une base de données fournies par un serveur intermédiaire. Cela permet donc de concevoir des sites web dynamiques. Nous éclaircisserons ce concept plus tard quand nous étudierons l'intégration d'Angular avec Django.

#########################
Spécificités et avantages
#########################

Qu'est-ce qui fait qu'AngularJS puisse se démarquer de ses concurrents ? En bref qu'elles sont ses fonctionnalités qui facilitent tant la vie des développeurs ? La première chose à connaitre sur AngularJS est qu'il encourage le modèle MVC - modèle, vue, controlleur. Il s'agit en fait de séparer dans notre application ces trois composants. Grossièrement, le modèle est la partie qui gère les données de l'application, la vue est ce qui sera affiché à l'utilisateur et le controlleur est la partie qui relie modèle et vue, c'est-à-dire qu'il va chercher les données dans les modèles pour les donner à la vue qui les affiche. Un principe très important dans ce motif de programmation et que le minimum de code logique doit se trouver dans les vues, elles doivent se contenter d'afficher car toutes les opérations se font idéalement dans les controlleurs ou les modèles. Intéressons aux quelques fonctionnalités principales d'AngularJS.

*****************
Du HTML expressif
*****************

Parlons d'abord des vues. Le premier "miracle" du framework est de transformer le HTML statique en un language expressif et dynamique. A la base le HTML est un syntaxe qui permet de structurer une page internet à l'aide de balises qui définissent leur contenu comme par exemple les balises qui délimitent des paragraphes, les tires ou encore des formulaires, des images, des vidéos, etc. Et tradionnellement on utilise un autre language comme Python ou autre pour transformer la page et y insérer notre contenu dynamique qui provient de la base de données, concrétement il pourrait s'agir d'afficher une liste d'articles dans la page. Si on travaille avec Django, on assignerait à une variable ``articles`` tous les articles avec une requête SQL - SQL est le language pour communiquer avec une base de données relationelles - et ferait ensuite une boucle dans la page HTML et on afficherait le titre et le contenu de chaque article.

.. code-block:: html
    
    <html>
        <head></head>
        <body>
            <div id="articles">
                {% for article in articles %}
                    <h1>{{ article.titre }}</h1>
                    <p>{{ article.contenu }}</p>
                {% endfor %}
            </div>
        <body>
    <html>

Les balises ``{% %}`` et ``{{ }}`` signifie simplement que ce n'est plus du HTML mais que du code Django est à l'intérieur et doit être executé par le serveur.

Avec AngularJS, le modèle est légérement différent. En effet il y a ce qu'on appelle des directives - c'est une nomenclature spécifique à Angular. Ce sont des balises ou des attributs HTML supplémentaires qui ont été crées par AngularJS et qui sont mis à disposition, on peut les ajouter à notre page et cela effectuera différentes actions selon la directive. Par exemple pour reprendre l'exemple des articles, pour faire une boucle, on utilise la directive ``ng-repeat``. On l'utilise comme un simple attribut HTML:

.. code-block:: html

    <html>
        <head></head>
        <body>
            <div id="articles" ng-repeat="article in articles">
                <h1>{{ article.titre }}</h1>
                <p>{{ article.contenu }}</p>
            </div>
        <body>
    <html>

A l'instar de Django, les doubles accolades signifient qu'on veut exécuter du code Angular/Javascript à l'intérieur. Ici on affiche simple une variable, mais on pourrait également faire un calcul et afficher le résultat: ``{{ 1 + 2 }}``.

Comme précédement, on a assigné une variable avec tous les articles, par contre cette fois la boucle se fait directement en utilisant la directive qui se confond avec la syntaxe HTML. A l'intérieur de l'attribut, il faut utiliser la syntaxe Angular pour faire les boucles ``article in articles``, on lui dit ici de parcourir la variable articles et d'utiliser comme variable temporaire ``article`` pour chaque article parcouru. Il existe beaucoup d'autre directives dans avec AngularJS pour réagir au click d'une souris sur un élément, pour afficher ou cacher des parties, tout ce qui concerne les fonctionnalités des formulaires, etc. Il est aussi possible de créer des propres directives personnalisées avec le comportement désiré. Créer ses propres directives permet soit d'avoir un code plus clair soit d'éviter la répétion, dans les deux cas cette fonctionnalité est très utile et puissante. 

C'est à cause de ces directives qu'on parle d'HTML expressif car en effet avec celles-ci le HTML ne décrit pas seulement le contenu, mais aussi le comportement de l'application web et sa manière de fonctionner. On sait ainsi clairement et rapidemment en regardant notre page HTML les fonctionnalités qu'on a implémenter sur celle-ci et c'est plus facile d'avoir une vue d'ensemble de son application.

**********************
Two-way data binding
**********************