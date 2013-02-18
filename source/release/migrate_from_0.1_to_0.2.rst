Migration guide from the 0.1 version to the 0.2 version
#######################################################

Mandatory breaking changes
**************************

Consequences of the switch from multi-languages to multi-contexts
=================================================================

	* The context configuration is done in a dedicated file (it's not in :file:`config.php` anymore). Two new keys
	  ``contexts`` and ``sites`` exists now, in addition to ``locales``
	* Every columns ``lang``, ``lang_common_id``, ``lang_is_main`` of the database have been renamed with ``context``
	* The new ``context`` columns are now larger, from 5 to 25 characters
	* The ``Translatable`` ``behaviour`` has been renamed ``Twinnable``
	* In the ``CRUD``, the ``context`` notion has been replaced by ``environment``, to prevent confusions (``context_relation`` -> ``environment_relation``, ``item_context`` -> ``item_environment``)
	* Every associated variables have been renamed, too

Changes in the pages API
========================

	* ``Model_Page->get_link()`` -> ``Model_Page->link()``
	* ``Model_Page->get_href()`` -> ``Model_Page->url()``
	* ``Model_Page::get_url()`` -> ``Tools_Url::page()``
	* Deleted ``Model_Page::get_url_absolute()``
	* Every methods now returns absolutes URLs

Changes in the events API
=========================

    * ``front.start`` now takes an array as argument, containing the ``url`` key

Procedure to follow
===================

    * Create the :file:`contexts.config.php` (based on the sample).
    * Change :file:`config.php` and add the ``novius-os`` key (based on the sample).

    * Search ``_lang``, replace it with ``_context`` (i.e. ``page_lang => page_context``)
    * Search ``Nos\Model_``, replace it with ``Nos\{{app}}\Model_`` (i.e. ``Nos\Model_Page => Nos\Page\Model_Page``, ``Nos\Model_Media => Nos\Media\Model_Media``)
    * Search ``Translatable``, replace it with ``Contextable``
    * Search  ``Model_Page->get_link()``, replace it with ``Model_Page->link()``
    * Search ``Model_Page->get_href()``, replace it with ``Model_Page->url()``
    * Search ``Model_Page::get_url()``, replace it with ``Tools_Url::page()``

    * Search ``Nos\Widget_Page_Selector``, replace it with ``Nos\Page\Renderer_Selector``
    * Search ``Nos\Widget_Media``, replace it with ``Nos\Renderer_Media``
    * Search ``Nos\Widget_``, replace it with ``Nos\{{Renderer_Version}}``


Changes in the applications
***************************

Metadata
========


* Launchers: the ``url`` key has been replaced by ``action`` (standard :ref:`nosAction <api:php/configuration/application/nosActions>` syntax)
* Launchers: the ``icon64`` key has been replaced by ``icon``

.. code-block:: php

	<?php

	return array(
	    'launcher_name' => array(
	        'url' => '{{your_url_here}}',
	    ),
	);

	// Replace with

	return array(
	    'launcher_name' => array(
	        'action' => array(
	            'action' => 'nosTabs',
	            'tab' => array(
	                'url' => '{{your_url_here}}',
	            ),
	        ),
	    ),
	);


* An application now proved a list of icons:

.. code-block:: php

    <?php

    return array(
        'icons' => array(
            64 => 'static/apps/noviusos_page/img/64/page.png',
            32 => 'static/apps/noviusos_page/img/32/page.png',
            16 => 'static/apps/noviusos_page/img/16/page.png',
        ),
    );

* A launcher without ``icon`` will use the 64-icon from its application
* A tab without ``iconUrl`` will use the 32-icon from its application
* An enhancer without ``iconUrl`` will use the 16-icon from its application


CRUD configuration
==================

* ``widget`` was renamed to ``renderer``


.. code-block:: php

    <?php

    return array(
        'field_name' => array(
            'widget' => 'Nos\Widget_Media',
            'widget_options' => array(),
        ),
    );

    // À remplacer par :
    return array(
        'field_name' => array(
            'renderer' => 'Nos\Renderer_Media',
            'renderer_options' => array(),
        ),
    );


"Full" 0.2 migration
********************


This section relies on the hypothetical existence of a ``lib_agenda`` application.


Appdesk
=======

Models have a new :file:`common` configuration file which contains:
* a ``data_mapping``
* a list of ``actions``

In :file:`appdesk.config.php` :

* Delete the ``selectedView`` and ``views`` keys (if you only have one view with no JS config file).
* Spot the main model of your appdesk (it's in the ``query.model`` key).
* Create the associated :file:`config/common/{{model_name}}.config.php` file
    * ``{{model_name}}`` relates to the name of the model, in lowercase and without the ``Model_`` prefix (for instance, ``Model_Page`` becomes ``page``)
    * ``Model_Page`` will relates to the :file:`config/common/page.config.php` file


.. note::

    Pay attention to have ``'hideContexts' => true,`` in your appdesk's configuration if you items are not ``Contextable``.


Data mapping
------------

``data_mapping`` is the merge of the ``dataset`` and ``appdesk.grid.columns`` keys.


.. code-block:: php
   :emphasize-lines: 6,11-21,29-49

    <?php

    // Old code of appdesk.config.php
    return array(
        'query' => array(
            'model' => 'Lib\Agenda\Model_Event',
            'order_by' => array('evt_date_begin' => 'DESC'),
            'limit' => 20,
        ),
        // ...
        'dataset' => array(
            'id'            => 'evt_id',
            'title'         => 'evt_title',
            'periode'       => array(
                'search_column' => 'evt_date_begin',
                'dataType'      => 'datetime',
                'value'         => function ($object) {
                    // ...
                },
            },
        ),
        // ...
        'appdesk' => array(
            // ...
            'grid' => array(
                'urlJson' => 'admin/lib_agenda/appdesk/json',
                'columns' => array(
                    'id' => array(
                        'headerText' => __('Id'),
                        'dataKey' => 'id'
                    ),
                    'title' => array(
                        'headerText' => __('Name'),
                        'dataKey' => 'title'
                    ),
                    'periode' => array(
                        'headerText' => __('Dates'),
                        'dataKey' => 'periode'
                    ),
                    'published' => array(
                        'headerText' => __('Status'),
                        'dataKey' => 'publication_status'
                    ),
                    'actions' => array(
                        'actions' => array('update', 'delete'),
                    ),
                ),
            ),
            // ...
        ),
    );


.. code-block:: php
   :emphasize-lines: 12,16

    <?php

    // New code in appdesk.config.php
    return array(
        'query' => array(
            'order_by' => array('evt_date_begin' => 'DESC'),
            'limit' => 20,
        ),
        // Tells what is the model, and wich 'common' file to load
        'model' => 'Lib\Agenda\Model_Event',
        // ...
        // MOVE / MERGE the 'dataset' key into
        // ...
        'appdesk' => array(
            // ...
            // MOVE / MERGE the 'grid' key into
            // ...
        ),
    );


.. code-block:: php
   :emphasize-lines: 5

    <?php

    // Code in the new 'event.config.php' file
    return array(
        // Merge of 'appdesk.dataset' and 'appdesk.grid.columns'
        'data_mapping' => array(
            'id' => array(
                'title' => __('Id'),
                'column' => 'evt_id'
            ),
            'title' => array(
                'title' => __('Name'),
                'column' => 'evt_title'
            ),
            'periode' => array(
                'title' => __('Dates'),
                'search_column' => 'evt_date_begin',
                'value'         => function ($object) {
                    // ...
                }
            ),
            'published' => array(
                'title' => __('Status'),
                'method' => 'publication_status'
            ),
        ),
    );

Some remarks:
* ``headerText`` can be written ``title`` (easier to remember, used in the natives apps)
* ``datakey`` can be written``column``
* ``value`` can still contain a callback function
* ``method`` is a new option which execute a methid instand of retrieving a ``column``



Actions
-------

Actions of the main model (the one on the grid) must also be moved in the :file:`common` file.

.. code-block:: php
   :emphasize-lines: 8-16

    <?php

    // Old code of appdesk.config.php
    return array(
        // ...
        'appdesk' => array(
            // ...
            // MOVE the 'actions' key into 'config/common/{{model_name}}.config.php'
            'actions' => array(
                'edit' => array(
                    // ...
                ),
                'delete' => array(
                    // ...
                ),
            ),
            // ...
        ),
        // ...
    );

.. code-block:: php
   :emphasize-lines: 8

    <?php

    // new code in appdesk.config.php
    return array(
        // ...
        'appdesk' => array(
            // ...
            // The 'actions' key is not here anymore
            // ...
        ),
        // ...
    );


.. code-block:: php
   :emphasize-lines: 9-17

    <?php

    // New code in 'config/common/event.config.php'
    return array(
        'data_mapping' => array(
            // ...
        ),

        // Configuration array moved from 'appdesk.actions'
        'actions' => array(
            'Lib\Agenda\Model_Event.edit' => array(
                // ...
            ),
            'Lib\Agenda\Model_Event.delete' => array(
                // ...
            ),
        ),
    );



From the moment the :file:`common` file  is used, the following generic actions appear:
* ``add``
* ``edit`` (and not ``update`` !)
* ``visualise`` (if appropriate, i.e. if the model has the ``Urlrenhancer`` behaviour)
* ``delete``
* ``share`` (if appropriate)


In our ``lib_agenda`` application, ``Model_Event`` had an ``update`` action, which now appeas twice... (because we used the wrong name ``update`` instead of ``edit``).

So, to fix it in our ``lib_agenda`` application, we need to:
* rename ``update`` to ``edit``
* Delete the ``edit`` and ``delete`` keys, since we're doing the default processing
* It's possible to keep only the keys we need to overwrite (to change the default texts for instance...)

Notes :
* In the Novius OS version used, it was needed to prefix actions by the model name, it's no longer necessary in the final version
* ``{{controller_base_url}}`` can be used in action's URLs. In the ``lib_agenda`` application, it's replaced by ``lib_agenda/admin/agenda/``
* A new ``targets`` keys allows to define where actions must appear (see comments).

.. code-block:: php

    <?php

    // Example of placeholder {{controller_base_url}} + 'targets'
    array(
        'Lib\Agenda\Model_Event.edit' => array(
            'action' =>
                'action' => 'nosTabs',
                'tab' => array(
                    'url' => "{{controller_base_url}}insert_update/{{id}}",
                    'label' => __('Modifier'),
                ),
            ),
            'label' => __('Modifier'),
            'primary' => true,
            'icon' => 'pencil',
            // New key to define where this action appear
            'targets' => array(
                'grid' => true, // In the grid (in the last 'actions' column)
                'toolbar-grid' => true, // On the appdesk, in the toolbar (previously configured using 'appdesk.button')
                'toolbar-edit' => true, // On the item form (top-right corner)
            ),
        )
    );


The default configuration for the targets is like below:
* ``grid`` : edit + visualise + delete
* ``toolbar-grid`` : add
* ``toolbar-edit`` : visualise + share + delete

.. note::

    For now, ``appdesk.appdesk.buttons`` is still defined, and has priority over the default configuration.
    Given we have 2 actions 'Add an event' and 'Add a category' from 2 different models, we can't delete it (for now).



I18n and translations
---------------------

Texts can be configured using the ``i18n`` key.

Please see the :file:`framework/config/common_i18n.config.php` to get the list of possible keys.

.. code-block:: php

    <?php

    return array(
        'i18n' => array(
            // Crud
            'notification item added' => __('And voilà! The page has been added.'),
            'notification item deleted' => __('The page has been deleted.'),

            // General errors
            'notification item does not exist anymore' => __('This page doesn’t exist any more. It has been deleted.'),
            'notification item not found' => __('We cannot find this page.'),

            // Blank slate
            'translate error parent not available in context' => __('We’re afraid this page cannot be added in {{context}} because its <a>parent</a> is not available in this context yet.'),
            'translate error parent not available in language' => __('We’re afraid this page cannot be added in {{language}} because its <a>parent</a> is not available in this language yet.'),

            // Deletion popup
            'deleting item title' => __('Deleting the page ‘{{title}}’'),

            # Delete action's labels
            'deleting button 1 item' => __('Yes, delete this page'),
            'deleting button N items' => __('Yes, delete these {{count}} pages'),

            '1 item' => __('1 page'),
            'N items' => __('{{count}} pages'),

            # Keep only if the model has the behaviour Contextable
            'deleting with N contexts' => __('This page exists in <strong>{{context_count}} contexts</strong>.'),
            'deleting with N languages' => __('This page exists in <strong>{{language_count}} languages</strong>.'),

            # Keep only if the model has the behaviours Contextable + Tree
            'deleting with N contexts and N children' => __('This page exists in <strong>{{context_count}} contexts</strong> and has <strong>{{children_count}} sub-pages</strong>.'),
            'deleting with N contexts and 1 child' => __('This page exists in <strong>{{context_count}} contexts</strong> and has <strong>one sub-page</strong>.'),
            'deleting with N languages and N children' => __('This page exists in <strong>{{language_count}} languages</strong> and has <strong>{{children_count}} sub-pages</strong>.'),
            'deleting with N languages and 1 child' => __('This page exists in <strong>{{language_count}} languages</strong> and has <strong>one sub-page</strong>.'),

            # Keep only if the model has the behaviour Tree
            'deleting with 1 child' => __('This page has <strong>1 sub-page</strong>.'),
            'deleting with N children' => __('This page has <strong>{{children_count}} sub-pages</strong>.'),
        ),
    );


Inspectors
----------

In the 0.1 version, the inspectors are configured in 3 different places:
* The ``appdesk.appdesk.inspectors`` key
* The ``inputs`` key
* The :file:`inspector/{{model}}.config.php` configuration file

In the 0.2 version, ``inputs`` must be moved into their corresponding :file:`inspector/{{model}}.config.php` file.



Category
^^^^^^^^

.. code-block:: php
   :emphasize-lines: 7-11

    <?php

    // Old code from appdesk.config.php
    return array(
        // ...
        'inputs' => array(
            // This input communicates with the filter for the category inspector
            // We move the key (evt_cat_id) into 'input.key' and the callback function into 'input.query'
            'evt_cat_id' => function($value, $query) {
                // ...
            },
        ),
        // ...
    );


.. code-block:: php

    <?php

    // New code in config/controller/admin/inspector/category.config.php
    return array(
        // ...
        'input' => array(
            'key' => 'evt_cat_id',
            'query' => function($value, $query) {
                // ...
            },
        ),
        // ...
    );


Date
^^^^

.. code-block:: php

    <?php

    // Old code from appdesk.config.php
    return array(
        // ...
        'appdesk' => array(
            'appdesk' => array(
                // ...
                'inspectors' => array(
                    // This array must be moved in the configuration file of the inspector, under a new 'appdesk' key
                    'startdate' => array(
                        'label' => __('Start date'),
                        'url' => 'admin/lib_agenda/inspector/date/list',
                        'inputName' => 'startdate',
                        'vertical' => true
                    ),
                    // ...
                ),
            ),
            // ...
        ),
    );


Here, the inspector doesn't have a configuration file yet, we need to create one:


.. code-block:: php

    <?php

    // New file config/controller/admin/inspector/date.config.php
    return array(
        'input' => array(
            'key' => 'evt_date_begin',
            // the 'qeury' keys is not needed, the date inspector will generate it automatically using the key
        ),

        // Old 'appdesk.appdesk.inspectors.startdate'
        'appdesk' => array(
            'label' => __('Start date'),
        ),
    );


The idea is to encapsulate the ``appdesk.appdesk.inspectors.{{inspector_name}}`` array inside an ``appdesk`` key of the
inspector's configuration file.

published
^^^^^^^^^

.. code-block:: php

    <?php

    // Old code from appdesk.config.php
    return array(
        // ...
        'inputs' => array(
            // ...
            'evt_published' => function($value, $query) {
                // ...
            },
        ),
        // ...
        'appdesk' => array(
            'appdesk' => array(
                // ...
                'inspectors' => array(
                    'published' => array(
                        'vertical' => true,
                        'url' => 'admin/lib_agenda/inspector/published/list',
                        'inputName' => 'evt_published',
                        'grid' => array(
                            'columns' => array(
                                'title' => array(
                                    'visible' => false,
                                    'dataKey' => 'title',
                                ),
                                'icon_title' => array(
                                    'headerText' => __('Status'),
                                    'dataKey' => 'icon_title',
                                ),
                                'id' => array(
                                    'visible' => false,
                                    'dataKey' => 'id',
                                ),
                            ),
                        ),
                    ),
                    // ...
                ),
                // ...
            ),
            // ...
        ),
    );


The ``published`` inspector already has its configuration file. Let's complete it with a new ``appdesk`` key:


.. code-block:: php
   :emphasize-lines: 8,16

    <?php

    // New file config/controller/admin/inspector/date.config.php
    return array(
        'data' => array(
            // ...
        ),

        // Old 'appdesk.appdesk.inspectors.published'
        'input' => array(
            'key' => 'evt_published',
            'query' => function($value, $query) {
                // ...
            },
        ),

        // Old 'input.evt_published'
        'appdesk' => array(
            'vertical' => true,
            'inputName' => 'evt_published',
            'url' => 'admin/lib_agenda/inspector/published/list',
            'grid' => array(
                'columns' => array(
                    'title' => array(
                        'visible' => false,
                        'dataKey' => 'title',
                    ),
                    'icon_title' => array(
                        'headerText' => __('Status'),
                        'dataKey' => 'icon_title',
                    ),
                    'id' => array(
                        'visible' => false,
                        'dataKey' => 'id',
                    ),
                ),
            ),
        ),
    );







