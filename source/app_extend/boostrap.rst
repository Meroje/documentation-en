Bootstrap
#########

The bootstrap file allows you to execute php code when the website / an application is loaded.
It can be placed in two locations :

* ``local/boostrap.php``: will be executed when the website is loaded.
* ``local/applications/APPLICATION/bootstrap.php``: will be executed when the application ``APPLICATION`` is loaded.

It is possible to use the bootstrap to extend an application. It is there :ref:`events <api:php/events>` and
:ref:`view redirects <api:php/classes/view>` can be used.