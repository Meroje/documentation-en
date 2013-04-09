Le bootstrap
############

Le fichier bootstrap permet d'exécuter du code php lors du chargement d'un site / d'une application.
Il est possible d'en créer à deux emplacements :

* ``local/bootstrap.php`` : s'éxecutera  lors du chargement du site.
* ``local/applications/APPLICATION/bootstrap.php`` : s'éxecutera lors du chargement de l'application ``APPLICATION``.

L'intérêt d'un tel fichier est qu'il permet de facilement étendre une application. C'est dans ces fichiers que l'on
pourra utiliser les :ref:`événements <api:php/events>` et les :ref:`redirections de vue <api:php/classes/view>`.