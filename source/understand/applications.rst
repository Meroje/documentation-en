.. index:: Application

Applications' fundamentals
==========================

An application is defined by its models, controllers and views. They vary upon the type of the application, but some
principles doesn't change and can be reused for every application.


Defining an application
-----------------------

For an application to be added (installed) using the application manager, it needs a :file:`metadata.config.php` file.
This file must contain the application's namespace, written as ``Provider\AppName``, along its name, version and provider
(at least a name).

An application should also defines one of the following in the :file:`metadata.config.php` file:

.. glossary::

	Launchers
		Icon on the home tab, used to launch an application.

	Enhancers
		They allow applications to enhance a WYSIWYG edited content.

	Templates
		Layout for the front-office.

	Data catchers
	    Component which allows an application to exploit shared data.


.. seealso::

    `'Understanding the applications' infographic <http://novius-os.github.com/docs/applications.html>`__


.. index:: App Desk

L’App Desk
----------

To understand the App Desk, please :doc:`read the ergonomic guidelines <ergonomy>` first.


App Desk's configuration
^^^^^^^^^^^^^^^^^^^^^^^^


.. image:: images/appdesk_ergonomy.png
	:alt: App Desk
	:align: center

The main feature of the AppDesk is several configurable elements:

- how to display the data ;
- the data itself ;
- main and secondary actions.

The :guilabel:`main grid` can offer several :guilabel:`views` (don't mistake them with the V of MVC) : grid, tree or thumbnails.

These views are defined with configuration files, which specify exactly which data to display and the :guilabel:`actions`.

:guilabel:`Inspecteurs` are also defined with configuration files. They tell on which attribute or relation the data of
the main grid will be filtered, and the actions associated to the meta items contained in the inspector. Please note the
inspectors either based on:

- The same model as the one of the main grid (like a publication date), the inspectors then uses a particular column /
  attribute of the item to filter the results;
- Another model (like posts' author), the inspectors then uses a relation fo filter the results.


Controllers, forms and models
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

On the App Desk screen, the modifications on the data are operated by calling a controller.

Some operations are made directly (for instance to delete an item, only a confirmation is asked). In this case, the
the App Desk's controller is responsible to carry them out.

Other operations needs a specific view and it's the controller of the item which is responsible. Most of the time, the
view is a form (new item / edition), which is built using a configuration file and is populated using an instance
of the associated Model. The controller is invoked again to save the data when submiting the form.

.. index:: Observers
.. index:: Behaviours

Observers and behaviours
------------------------

Observers exists in the `FuelPHP framework <http://dev-docs.fuelphp.com/packages/orm/observers/intro.html>`__.

They contain the logic which is directly dependent of the model. They're expressed when a specific event is triggered.
They are used to format, change or validate the properties of the model (for instance before adding a new item in the
database).

Behaviours are specific to Novius OS, they embrace and extend this principle. Observers can only make specific actions
when the model triggers a specific event (such as ``before_save``). Behaviours can do more and contain a set of
additional methods which defines a particular behaviour, for example translatable or publishable. They also can be
triggered by events.

The main advantage of this tools is to share code between different models (horizontal reuse).

.. seealso::

    `API documentation <api:php/behaviours/index>`__
