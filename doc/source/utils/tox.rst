===
Tox
===

Tox est un outil qui permet d'encapsuler une logique de test spécifique, ainsi que d'executer cette logique dans un ou plusieurs environnements. Grace à cette encapsulation, on pourra donc demander à un serveur d'integration (travis dans notre cas) de lancer ladite logique, et/ou plusieurs en parallèle.

Pour le developpeur, lancer les tests à partir de tox permet de s'asssurer que les tests sont lancés avec les tests sont lancés avec les bonnes dépendances. Lorsque vous construisez votre ``virtualenv`` vous meme, rien de vous assure que vos dépendances sont le reflet de vos requirements au moment du lancement des tests.

Les commandes de test sont donc centralisées et plus simple à écrire dans la console, il en existe actuellement plusieurs disponibles.

.. attention::

    Pour lancer les commandes tox, vous devez sortir de votre environnement virtualenv si vous en avez un (ce qui vous permet de lancer certaines commandes via ``sudo`` ). Ce qui signifie que tox doit etre installé en dehors de n'importe quel environnement de developpement.

Les tests back-end
------------------

Tests avec une base Sqlite
~~~~~~~~~~~~~~~~~~~~~~~~~~

Les tests back-end en mode developpement sont des tests lancés sur le back-end en se basant sur le fichier ``settings.py``. Ce qui signifie donc qu'il se base sur la base de donnée que vous avez définit dans le fichier ``settings.py``. Si vous avez un fichier ``settings_prod.py`` se dernier sera aussi utilisé pour surcharger les variables du ``settings.py``.

La commande de lancement est la suivante :

.. sourcecode:: bash

    tox -e back

Si vous souhaitez uniquement lancer les tests d'un module du projet, le module article par exemple :

.. sourcecode:: bash

    tox -e back zds.article

ça fonctionne donc exactement comme avec ``python manage.py zds.article``.

Tests avec une base MySQL
~~~~~~~~~~~~~~~~~~~~~~~~~

Les tests back-end en mode production sont des tests lancés sur le back-end en se basant aussi sur le fichier ``settings.py``. La différence avec les tests ci-dessus est que vous pourrez ici vous servir d'une base de donnée MySQL (en surchargeant le dictionnaire ``DATABASES`` dans ``settings_prod.py``) car tox va rajouter la dépendances ``MySQL-python`` qui permet de se connecter à une base de donnée MySQL.

Tox inclut aussi ``ujson`` la librairie utilisée en production pour lire les fichiers json de manière optimisée. Notons tout de meme que cette commande vérifie aussi avec ``flake8`` que le code est conforme à la pep8.

La commande de lancement est la suivante :

.. sourcecode:: bash

    tox -e back_mysql

Les tests front-end
-------------------

Les tests front-end se contentent, après avoir mis à jour votre version de npm, de lancer les commande de tests du front-end. La commande lance donc les commandes suivantes :

.. sourcecode:: bash

    npm install npm -g
    npm install
    npm run travis

Cette logique est donc encapsulée dans la commande suivante (le ``sudo`` est important ici car la commande fait la mise à jour npm) :

.. sourcecode:: bash

    sudo tox -e front

Les tests de la documentation
-----------------------------

Pour tester la génération de la documentation en html, la commande est la suivante : 


.. sourcecode:: bash

	tox -e docs -- html


Cette commande n'installera que les dépendances necessaires à savoir ``Sphinx``.

Les tests de style pep8
-----------------------

Pour tester le style pep8 uniquement, la commande est la suivante : 


.. sourcecode:: bash

	tox -e flake8