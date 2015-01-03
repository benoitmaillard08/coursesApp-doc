===========
RestLess
===========

#############
Présentation
#############

`RestLess <https://github.com/dobarkod/django-restless>`_ est un set d'outils permettant de faciliter l'implémentation d'une API JSON dans Django. Il a l'avatange d'être léger et facile à utiliser comme nous le verrons par la suite. Une API - Application Programming Interface - est basiquement une application qui offre des services accessible par une autre application et le JSON est un format de données dans le style d'un dictionnaire ``{"nom" : "Keran", "prenom" : "Kocher"}``. On appelle donc une API JSON, une application qui fournit des données en format JSON. Concrétement il s'agit d'une série d'URLs qui fournissent le contenu de différentes tables de la BD en format JSON. Quand on se rend sur une de ces URLs, on ne voit pas une page HTML comme on a l'habitude de voir, mais simplement le dictionnaire de données affiché. Sur notre site web rendez-vous sur http://webmath.com/courses/api/themes pour voir à quoi cela ressemble.

Dans la présentation concernant AngularJS, nous avons vu que le framework n'était pas capable de communiquer directement avec une base de données, c'est une limitation de JavaScript, qui s'exécute côté client - sur l'ordinateur de l'utilisateur, opposé de côté serveur - dans notre cas. Pour palier ce problème on utilise donc un language intermédiaire qui est capable de communiquer avec une BD et qui s'exécute côté serveur, c'est le cas de Python et son framework Django. Django va donc chercher les données dans la BD, les transforme en format JSON puis les sert. Il suffit ensuite pour AngularJS d'accéder à la bonne URL et elle dispose ensuite des données qu'on peut utiliser à notre guise.

********************
Les vues génériques
********************

Quand on développe une application pour web, il arrive souvent d'avoir du code redondant, répétitif. En effet quand on crée des fonctionnalités, il s'agit souvent d'avoir une table dans la base de données puis d'interréagir avec celle-ci ensuite pour modifer les données. Et ces interractions sont généralement toujours les mêmes, on parle généralement du CRUD: create, read, update and delete ou en français créer, lire, mettre à jour et supprimer. On aura donc une table et on voudra accomplir les opérations CRUD dessus et pour ce faire on aura une série d'URLs et d'actions derrières. Prenons l'exemple de la création d'un blog tout à fait typique: on va créer une table ``articles`` et on va implémenter les actions suivantes: on veut pourvoir afficher tous les articles ou pouvoir en afficher un seul - read, ajouter un nouvel article - create, le mettre à jour - update - ou le supprimer - delete. Et voilà, avec ces 4 opérations on peut disposer d'un blog complet et fonctionnel. Et pour beaucoup de fonctionnalité il s'agira d'effectuer toujours ces mêmes opérations classiques, imaginez par exemple un système de commentaires ou alors d'événements. Pour programmer ces outils basiques et conventionnels deux moyens sont à notre disposition dans Django. Soit on programme de A à Z les opérations, avec Django il s'agit basiquement de créer une URL et lui assigner une fonction qui retourne du code HTML et qui s'occupe de communiquer avec la base de données. Si on utilise cette méthode, on risque de devoir programmer souvent le même code au fil du développement et de perdre du temps. Ci-dessous le code pour afficher tous les articles avec la méthode normale.

.. code-block:: python
    
    # url.py - fichier qui gère les URLs du site

    from django.conf.urls import patterns, url, include

    from courses import views

    # on crée une URL /articles qui utilise la fonction index - voir fichier views.py
    urlpatterns = patterns('',
        url(r'^articles$', views.index),
    )

    # views.py - fichier qui contient les fonctions liées aux URLs

    from django.shortcuts import render

    # fonction reliée à /articles
    def index(request):
        # Récupère tous les articles de la BD (fait appelle au modèle Article)
        articles = Article.objects.all()
        # Retourne le code HTML en utilisant le ficher courses.html
        return render(request, "courses/courses.html", locals())


La deuxième méthode consiste à utiliser les vues génériques. Ce sont en effet des classes que Django possède contenant déjà les opérations conventionnelles déjà écrites. Il nous suffit donc de créer notre propre classe qui hérite des classes Django, des vues génériques et d'ensuite la relier à notre URL comme on le faisait avec la fonction. Pourquoi créer une classe et ne pas utiliser directement les classes Django ? Tout simplement pour pouvoir personnaliser la classe. Il faut déjà obligatoirement spécifier le modèle que doit utiliser la classe par exemple pour savoir quelles enregistremens elle doit aller récupérer. Voici donc la même fonctionnalité qu'avant mais avec les vues génériques.

.. code-block:: python
    
    # url.py - fichier qui gère les URLs du site

    from django.conf.urls import patterns, url, include

    from courses.views import ArticlesList

    # on crée une URL /articles qui utilise la vue générique ArticlesList - voir fichier views.py
    urlpatterns = patterns('',
        url(r'^articles$', ArticlesList.as_view()),
    )

    # views.py - fichier qui contient les fonctions liées aux URLs

    from django.shortcuts import render
    from django.views.generic import ListView

    # la classe générique reliée à /articles
    class ArticlesList(ListView):

        # on spécifie le modèle à utiliser
        model = Article

Avec la seconde méthode le code est plus concis. L'exemple montre comment générer une liste d'articles, mais il existe une classe pour chaque opération du CRUD. Il est encore possible de personnaliser notre classe ``ArticlesList`` avec des options ou en surchargeant les méthodes. Par contre évidement que si notre fonctionnilité a des besoins spécifiques qui s'éloignent trop de la convention, les vues génériques ne sont plus adaptées car leur personnalisation a évidement des limites. Dans ces cas-ci on retourne à la première méthode.

********************
Concept de RestLess
********************

############
Application
############
