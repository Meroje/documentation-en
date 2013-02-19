``Contextable`` / ``Twinnable``
###############################

Although Novius OS natively manages multiple contexts, each application decides the way it uses them on its own.

Three cases can be found.

Application not using any context
*********************************

It's the most simple and default case. The application doesn't manage any context and has nothing to do.

Its content will be the same across all contexts and can be used (through :term:`enhancers`) by any of them.

.. index:: Contextable

``Contextable`` application
***************************

The application manages contexts. Each item will be associated with a context, and should only be used inside of it.

Technically, tables of the application have a ``context`` column (of type ``varchar(25)``) which contains the context
code. :ref:`Models <api:php/models/model>` of the application must implement the :ref:`api:php/behaviours/contextable`
behaviour.

.. index:: Twinnable

``Twinnable`` application
*************************

The application manages contexts and can link them together.
Each item is associated with a context and can be linked to other items from a different context.

Technically, tables of the application have 3 columns:

:context: 			 ``varchar(25)``, contains the context code of the item.
:common_id_property: ``int`` contains a common ID share among all linked items.
:is_main_property:   ``boolean``, each group of linked items will only have one main item.

:ref:`Models <api:php/models/model>` of the application must implement the :ref:`api:php/behaviours/twinnable`
behaviour.

Example
=======

Schema of our example table:

* item_id (primary key)
* item_context
* item_common_id_property
* item_is_main_property
* item_title

Let's create a first item:

=======	============ ======================= ===================== ======================
item_id	item_context item_common_id_property item_is_main_property item_title
=======	============ ======================= ===================== ======================
1       main::fr_FR  1                       1                     Premier item
=======	============ ======================= ===================== ======================

| The ``item_common_id_property`` column is assigned with the same value as the primary key.
| The item is primary, and the ``item_is_main_property`` is set to ``1``.


Let's add another item in another context, and linked to the first one:

=======	============ ======================= ===================== ======================
item_id	item_context item_common_id_property item_is_main_property item_title
=======	============ ======================= ===================== ======================
1       main::fr_FR  1                       1                     Premier item
2       main::en_GB  1                       0                     First item
=======	============ ======================= ===================== ======================

| The ``item_common_id_property`` column is assigned with the same ``item_common_id_property`` to which it's linked to.
| ``item_is_main_property`` is set to ``0``, it's not the primary item.

Let's see how it looks after a few more addition:

=======	============ ======================= ===================== ======================
item_id	item_context item_common_id_property item_is_main_property item_title
=======	============ ======================= ===================== ======================
1       main::fr_FR  1                       1                     Premier item
2       main::en_GB  1                       0                     First item
3       main::en_GB	 3                       1                     Second item
4       main::fr_FR	 3                       0                     Second item (fr)
5       event::fr_FR 5                       1                     Item du site event
6       main::es_ES	 1                       0                     First item (es)
=======	============ ======================= ===================== ======================

| The items with ID ``1``, ``2`` and ``6`` are linked together, and the main item is ``1`` / ``main::en_GB``.
| The items with ID ``3`` and ``4`` are linked together, and the main item is ``3`` / ``main::en_GB``.

Let's delete the item with ID ``1`` :

=======	============ ======================= ===================== ======================
item_id	item_context item_common_id_property item_is_main_property item_title
=======	============ ======================= ===================== ======================
2       main::en_GB  *1*                     **1**                 First item
3       main::en_GB	 3                       1                     Second item
4       main::fr_FR	 3                       0                     Second item
5       event::fr_FR 5                       1                     Item du site vent
6       main::es_ES	 *1*                     0                     First item
=======	============ ======================= ===================== ======================

The item with ID ``2`` now becomes the main item, but the ``item_common_id_property`` has **not** changed.

