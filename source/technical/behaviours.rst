Behaviours
==========

``Behaviours`` are meant to extend ``Orm\Model`` features in a re-usable way.

They are similar to `Observers <http://docs.fuelphp.com/packages/orm/observers/intro.html>`_ but more powerful.

Like ``Observers``, they have options to be configured.

Like ``Observers``, they can catch events to act on a model (for example, ``before_save``).

In addition, they also provide a set of new methods on the instantiated ``Model`` item. Sometimes they also provide new events.


* :ref:`behaviours_publishable`
* :ref:`behaviours_sortable`
* :ref:`behaviours_translatable`
* :ref:`behaviours_tree`
* :ref:`behaviours_urlenhancer`
* :ref:`behaviours_virtualname`
* :ref:`behaviours_virtualpath`
* Sharable: see :doc:`Sharing and data catchers <sharing>`


:ref:`behaviours_printer-friendly`


.. _behaviours_publishable:

Publishable
-----------

*Note: only a yes/no state is supported for now. Full publication start & end date will come later.*

1. Column
^^^^^^^^^

* ``publication_bool_property [bool]``: which column is used to store the state of publication (yes/no)

2. Configuration
^^^^^^^^^^^^^^^^

The publishable behaviour doesn't have options.

3. Methods provided on an item
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``(bool) published()``
""""""""""""""""""""""

Returns whether the item is published or not

4. Example
^^^^^^^^^^

.. code-block:: php

	<?php
	class Model_Page extends \Nos\Orm\Model
	{
		protected static $_behaviours = array(
			'Nos\Orm_Behaviour_Publishable' => array(
				'publication_bool_property' => 'page_published',
			),
		);
	}

	$page = Model_Page::find(1);

	if ($page->published()) {
		// Do something
	}

.. _behaviours_sortable:

Sortable
--------

1. Column 
^^^^^^^^^

* ``sort_property [double]``: which column is used to store the sort order / rank

2. Configuration
^^^^^^^^^^^^^^^^

* ``sort_order``: ASC (default) or DESC 

3. Methods provided on an item
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``move_before($item)``
""""""""""""""""""""""

``move_after($item)``
"""""""""""""""""""""

``move_to_last_position()``
"""""""""""""""""""""""""""

4. Example
^^^^^^^^^^

.. code-block:: php

	<?php
	class Model_Page extends \Nos\Orm\Model
	{
		protected static $_behaviours = array(
			'Nos\Orm_Behaviour_Sortable' => array(
				'events' => array('after_sort', 'before_insert'),
				'sort_property' => 'page_sort',
			),
		);
	}

	$page_1 = Model_Page::find(1);
	$page_2 = Model_Page::find(2);

	$page_2->move_after($page_1);



.. _behaviours_translatable:

Translatable
-----------------------------------------------------

This behaviour needs 3 columns in the table to operate properly.

1. Columns
^^^^^^^^^^

* ``common_id_property [int]``: which column is used to store the common ID
* ``single_id_property [int]``: which column is used to store the single ID (equals to common ID for the main lang, ``null`` otherwise)
* ``lang_property [varchar(5)]``:  which column is used to store the locale code (examples: ``en_GB`` or ``fr_FR``)

2. Configuration
^^^^^^^^^^^^^^^^

* ``default_lang``: default lang to use when not specified (for example upon creation)

3. Methods provided on an item
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``get_lang()``
""""""""""""""

Returns the current lang of the item.

``is_main_lang()``
""""""""""""""""""

Returns true or false.

``find_main_lang()``
""""""""""""""""""""

Returns the item in the main lang, ``null`` otherwise.

``find_lang($lang)``
""""""""""""""""""""

Possible values for ``$lang``:

* ``lang_COUNTRY``: returns the item in the appropriate language, if it exists, ``null`` otherwise.
* ``all``: returns an array of all versions of this item.
* ``main``: returns the item in the main lang. Alias for ``find_main_lang()``.

``get_all_lang()``
""""""""""""""""""

Returns an array of all the langs of the item. Key is the item ID. Value is the locale code.

.. code-block:: php

	<?php array(
		2 => 'en_GB',
		5 => 'fr_FR',
	);

``get_other_lang()``
""""""""""""""""""""

Same as ``get_all_lang()``, but it doesn't include the current item lang.

4. Example
^^^^^^^^^^

.. code-block:: php

	<?php
	class Model_Page extends \Nos\Orm\Model
	{
		protected static $_behaviours = array(
			'Nos\Orm_Behaviour_Translatable' => array(
				'events' => array('before_insert', 'after_insert', 'before_save', 'after_delete', 'change_parent'),
				'lang_property'      => 'page_lang',
				'common_id_property' => 'page_lang_common_id',
				'single_id_property' => 'page_lang_single_id',
			),
		);
	}



.. _behaviours_tree:

Tree
-------------------------------------

1. Columns
^^^^^^^^^^

* ``level_property [int]``:  optional. Which column is used to store nesting level (depth).

2. Configuration
^^^^^^^^^^^^^^^^

* ``parent_relation``: relation used to fetch the parent.
* ``children_relation``: relation used to fetch the children.

3. Methods provided on an item
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``get_parent()``
""""""""""""""""

Returns the parent of this item, if it exists, ``null`` otherwise.

``set_parent($new_parent)``
"""""""""""""""""""""""""""

Sets a new parent for the item.

Can throw an Exception  if the item is being moved inside its own sub-tree.

If the item is ``Translatable`` and exists in multiple languages, all languages will be moved synchronously. This can throw an Exception if the new parent doesn't exists in one of the languages of the moved item.

``find_children($where = array(), $order_by = array(), $options = array())``
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

Returns all direct children of the item, optionally filtered and ordered by the parameters.

The method uses native's Fuel ``find()`` method, passing along the ``$options`` parameters as follow:

.. code-block:: php

	<?php
	$options = \Arr::merge($options, array(
		'where'    => $where,
		'order_by' => $order_by,
	));


``find_children_recursive($include_self)``
""""""""""""""""""""""""""""""""""""""""""

Returns all children and sub_children of the item.

* ``$include_self``: Should the returned array contain the item itself?

``find_root()``
"""""""""""""""

Returns the highest parent in the tree, or ``null`` if the item has no parent.

4. Example
^^^^^^^^^^

.. code-block:: php

	<?php
	class Model_Page extends \Nos\Orm\Model
	{
		protected static $_behaviours = array(
			'Nos\Orm_Behaviour_Tree' => array(
				'events' => array('before_query', 'after_delete'),
				'parent_relation' => 'parent',
				'children_relation' => 'children',
				'level_property' => 'page_level',
			),
		);

		protected static $_has_many = array(
			'children' => array(
				'key_from'       => 'page_id',
				'model_to'       => 'Nos\Model_Page',
				'key_to'         => 'page_parent_id',
				'cascade_save'   => false,
				'cascade_delete' => false,
			),
		);

		protected static $_belongs_to = array(
			'parent' => array(
				'key_from'       => 'page_parent_id',
				'model_to'       => 'Nos\Model_Page',
				'key_to'         => 'page_id',
				'cascade_save'   => false,
				'cascade_delete' => false,
			),
		);

	}





.. _behaviours_urlenhancer:

Urlenhancer
-----------

This behaviour provides helper URL methods relative to models which are displayed by :doc:`URL Enhancers <enhancers>`.

1. Column
^^^^^^^^^

This behaviour doesn't need columns.

2. Configuration
^^^^^^^^^^^^^^^^

* ``enhancers [array of strings]``: array of enhancer names able to generate an URL for this item.

The enhancers listed here must define a ``get_url_model($item, $params)`` method. See the :doc:`related documentation <enhancers>` for more details.

3. Methods provided on an item
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

All 3 methods take the same ``$params`` array:

* ``preview``: true or false. Whether to include unpublished pages. You have to include ``?_preview=1`` yourself.

``urls($params = array())``
"""""""""""""""""""""""""""

Returns an array of all available URL for this item. The array contains:

.. code-block:: php

	<?php
	array(
		'page_id::item_slug' => 'full_url (relative to base)',
	);


This way, we get all the informations we want:

* The page ID
* The generated URL by the enhancer (item slug)
* The current full URL (current page URL + enhancer part)

If there's no result, the function will return empty ``array()``

``url_canonical($params = array())``
""""""""""""""""""""""""""""""""""""

Apply only to items which have the ``Sharable`` behaviour. Returns the URL configured manually in the shared data (content nugget).

If the item is not shareable or has no URL configured manually, it's equivalent to ``$item->url();``

``url($params = array())``
""""""""""""""""""""""""""

Returns a valid URL for the item, or ``null`` of the item is not displayed.

4. Example
^^^^^^^^^^

.. code-block:: php

	<?php
	class Model_Monkey extends \Nos\Orm\Model
	{
		protected static $_behaviours = array(
			'Nos\Orm_Behaviour_Urlenhancer' => array(
				'enhancers' => array('noviusos_monkey'),
			),
		);
	}



.. _behaviours_virtualname:

Virtual name
------------

This will use the ``title_property`` of the model to auto-generate the virtual name (slug).

Upon ``save()``, an Exception will be throwed for item that does not fulfill the ``unique`` requirement.

1. Column
^^^^^^^^^

* ``virtual_name_property [varchar]``: which column is used to store the virtual name.

2. Configuration
^^^^^^^^^^^^^^^^

* ``unique``: true / false or 'lang'

3. Methods provided on an item
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``virtual_name()``
""""""""""""""""""

Returns the virtual name of the item.

4. Example
^^^^^^^^^^

.. code-block:: php

	<?php
	class Model_Monkey extends \Nos\Orm\Model
	{
		protected static $_behaviours = array(
			'Nos\Orm_Behaviour_Virtualname' => array(
				'events' => array('before_save', 'after_save'),
				'virtual_name_property' => 'monk_virtual_name',
			),
		);
	}



.. _behaviours_virtualpath:

Virtual path
------------

This is an extension of the ``Virtual name`` behaviour.

1. Columns
^^^^^^^^^^

* ``virtual_name_property [varchar]``: inherited from `Virtual name`
* ``virtual_path_property [varchar]``: which column is used to store the virtual path.

2. Configuration
^^^^^^^^^^^^^^^^

* ``unique``: inherited from ``Virtual name``
* ``extension[before]``: String to use to prepend extension
* ``extension[after]``: String to use to append extension
* ``extension[property]``: column to use to fetch the extension of the virtual path (slug).
* ``parent_relation``: Relation used to generate the first part of the virtual path (slug).

3. Methods provided on an item
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``virtual_path()``
""""""""""""""""""

Returns the virtual path of the item.

4. Example
^^^^^^^^^^

.. code-block:: php

	<?php
	class Model_Page extends \Nos\Orm\Model
	{
		protected static $_behaviours = array(
			'Nos\Orm_Behaviour_Virtualpath' => array(
				'events' => array('before_save', 'after_save', 'change_parent'),
				'virtual_name_property' => 'page_virtual_name',
				'virtual_path_property' => 'page_virtual_url',
				'extension_property' => '.html',
				'parent_relation' => 'parent',
			),
		);
	}


---------------------------------------------------------------------------------------------


.. _behaviours_printer-friendly:

Printer friendly
----------------

Publishable
^^^^^^^^^^^

* ``publication_bool_property [bool]``
* 
* ``(bool) published()``

Sortable
^^^^^^^^

* ``sort_property [double]``
* ``sort_order``
* 
* ``move_before($item)``
* ``move_after($item)``
* ``move_to_last_position()``

Translatable
^^^^^^^^^^^^

* ``common_id_property [int]``
* ``single_id_property [int]``
* ``lang_property [varchar(5)]``
* ``default_lang``
* 
* ``get_lang()``
* ``is_main_lang()``
* ``find_main_lang()``
* ``find_lang($lang)``: ``lang_COUNTRY``, 'all' or 'main'
* ``get_all_lang()``
* ``get_other_lang()``

Tree
^^^^

* ``parent_relation``
* ``children_relation``
* ``level_property``
* 
* ``get_parent()``
* ``set_parent($new_parent)``
* ``find_children($where = array(), $order_by = array(), $options = array())``
* ``find_children_recursive($include_self)`` 
* ``find_root()``

Urlenhancer
^^^^^^^^^^^

* ``enhancers[name]``
* 
* ``urls($params = array())``
* ``url_canonical($params = array())``
* ``url($params = array())``

Virtual name
^^^^^^^^^^^^

* ``virtual_name_property [varchar]``
* ``unique``: true / false or 'lang'
* 
* ``virtual_name()``


Virtual path
^^^^^^^^^^^^

* ``virtual_name_property [varchar]``
* ``virtual_path_property [varchar]``
* ``unique``: inherited from ``Virtual name``
* ``extension[before]``: String to use to prepend extension
* ``extension[after]``: String to use to append extension
* ``extension[property]``: column to use to fetch the extension of the virtual path (slug).
* ``parent_relation``: Relation used to generate the first part of the virtual path (slug).
* 
* ``virtual_path()``
