Software's fundamentals
#######################

.. index:: MVC

MVC structure
*************

Novius OS follows the `Model-View-Controller <http://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller>`__ pattern,
which defines how to work:

- when designing applications;
- when organising a project running Novius OS.

Frameworks usage
****************

Framework usages influences us a lot when designing and implementing applications, and it's really helpful to know the
role they play in. However, this documentation relates to Novius OS, so please refers to external documenations and
tutorials for more informations about them.

.. index:: FuelPHP

FuelPHP
=======

`FuelPHP <http://fuelphp.com>`__ is the PHP framework used by Novius OS.

The most used parts are the ORM, the cascading file system and the data validation. The framework also provides a lot of tools
which make application development easier, like the  `Arr <http://docs.fuelphp.com/classes/arr.html>` class for example.


.. index:: ORM

FuelPHP's ORM
=============

ORM stands for `Object-relational mapping <http://en.wikipedia.org/wiki/Object-relational_mapping>`__.

| The ORM allows to handle the database using PHP objects (and classes), allowing to express relations between them.
| An example is worth a thousand words:

.. code-block:: php

	$new_monkey = Model_Monkey::forge();
	$new_monkey->monk_name = 'Julian';
	$new_monkey->save();

	$monkeys = Model_Monkey::find('all');
	foreach ($monkeys as $monkey) {
		//...
	}

	$monkey = Model_Monkey::find(4);
	$monkey->delete();

Novius OS relies on `FuelPHP's ORM <http://www.fuelphp.com/docs/packages/orm/intro.html>`__. Please refer to its documentation.

| But Novius OS adds another notable feature to the ORM: ``Behaviours``.
| ``Behaviours`` allows to extends ``Model`` in a standardised and reusable way.

They're similar to FuelPHP's `Observers <http://docs.fuelphp.com/packages/orm/observers/intro.html>`__ but are more powerful:

* As ``Observers``, they have configurable options.
* As ``Observers``, they can intercept events to take action on the ``Model`` (such as ``before_save``, trigerred before updating the database).
* In addition, they also provide methods (static or instance) on the ``Model``.
* They also can provide new events.

jQuery UI / Wijmo
=================

Although actions are done server side using PHP, Novius OS is mostly written in JavaScript. This is explained by the
utmost importance given to the UI and the UX (see :doc:`ergonomy`).

To offer rich interfaces and interactions, Novius OS uses several JS frameworks:

.. glossary::

	jQuery
		| This framework makes HTML manipulation and DOM much simpler. It's not especially UI-oriented.
		| `Documentation <http://api.jquery.com/>`__

	jQuery UI
		| Built on top of jQuery to provide inferface interactions. Most of the Novius OS UI comes from this framework.
		| `Documentation <http://api.jqueryui.com/>`__

	Wijmo
		| Built on top of jQuery UI. Provides additional rich-interface elements, called widgets.
		| `Documentation <http://wijmo.com/wiki/index.php/Main_Page>`__ and `Exemples <http://wijmo.com/demo/explore/>`__


There's a hierarchy between those frameworks, Wijmo has the most impact on Novius OS' ergonomy.
