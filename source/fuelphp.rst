FuelPHP fundamentals
====================

What is MVC?
------------

MVC stands for Model-View-Controller.


MVC is an approach to separating your code depending on what role it plays. Basically, a request is handled by the **controller**.
It retrieves data using **models**. Then, it decides what **view** to use to display the data to your visitors.

.. seealso:: `MVC in the FuelPHP’s documentation <http://fuelphp.com/docs/general/mvc.html>`__

.. seealso:: `MVC in Wikipedia <http://en.wikipedia.org/wiki/Model–view–controller>`__


Where to create my new files?
-----------------------------

Every classes follows the same precise naming convention:

* lowercase, except the first letter of each level in uppercase ;
* underscores are use to separate directories.

For instance, the :file:`classes/controller/admin/login.php` file contains the class named ``Controller_Admin_Login``.

PHP `classes <http://fuelphp.com/docs/general/classes.html>`__ are placed in the :file:`classes` directory.

`Controllers <http://fuelphp.com/docs/general/controllers/base.html>`__ classes are found in the :file:`classes/controller` directory.

`Models <http://fuelphp.com/docs/general/models.html>`__ classes belongs in the :file:`classes/model` directory.


How to write a view?
--------------------

Views should be put in the :file:`views` directory.

.. seealso:: `FuelPHP’s documentation on views <http://fuelphp.com/docs/general/views.html>`__


How to use the ORM?
-------------------

An ORM does 2 things:

* it maps your database table rows to PHP objects ;
* it allows to establish relations between them.

FuelPHP’s ORM uses the `Active Record <http://en.wikipedia.org/wiki/Active_record_pattern>`__ pattern.

The following links from the FuelPHP’s documentation will help you:

.. seealso:: `Creating models <http://fuelphp.com/docs/packages/orm/creating_models.html>`__

.. seealso:: `Make DB queries based on these models <http://fuelphp.com/docs/packages/orm/crud.html>`__

.. seealso:: `Define relatons and use them in the DB queries <http://fuelphp.com/docs/packages/orm/relations/intro.html>`__



