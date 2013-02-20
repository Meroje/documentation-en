Differences with FuelPHP
########################

Constants path
**************

See :ref:`API documentation for constants <api:php/constants>`.

Autoloader
**********

Two ``namespaces`` are added by Novius OS :

:novius-os: points to :ref:`NOSPATH <api:php/constants/NOSPATH>`.
:local: points to :ref:`APPPATH <api:php/constants/APPPATH>`.

Bootstrap and entry points
**************************

In Novius OS, front-office and back-office are two very separated areas.

So instead of a single :file:`index.php` entry point from FuelPHP, Novius OS has two entry points:

* :file:`~/novius-os/htdocs/admin.php`: Back-office entry point. Handles all URL starting with :file:`/admin/`.
* :file:`~/novius-os/htdocs/front.php`: Front-office entry point. Handles all URL ending with ``.html`` or the root of your website.

Back-office entry point
=======================

Novius OS defines a special ``route`` for the back-office, which works as follow:

.. code-block:: php

	<?php
	'routes' => array(
		'^admin/(:segment)/(:any)' => '$1/admin/$2',
	),



For example, the URL :file:`admin/noviusos_page/page/insert_update/113` will be transformed into :file:`noviusos_page/admin/page/insert_update/113`.
Which boils down to executing the ``action_insert_update`` method from the ``Controller_Admin_Page`` ``controller`` of the ``noviusos_page`` application.

.. seealso::

	`FuelPHP's documentation about routing <http://fuelphp.com/docs/general/routing.html>`__.
