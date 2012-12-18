CRUD controller
===============

The CRUD controller is in charge of generating the forms (add, edit & delete) related to an item / model and handling automatically the multilingual / translation problematic.

Configuration
-------------

The configuration file looks like this:

.. code-block:: php

	<?php
	return array(
		'controller_url'  => 'admin/noviusos_monkey/monkey',
		'model' => 'Nos\\Monkey\\Model_Monkey',
		'messages' => array(),
		'tab' => array(),
		'layout' => array(),
		'fields' => array(),
	);


The ``controller_url`` key should tell where your controller is located. Here it's in the ``noviusos_monkey`` application / namespace, and it's name is ``Controller_Admin_Monkey``.

The ``model`` key should tell which model it generates forms for.

The ``messages`` key contains all generic messages used by CRUD controller.

The ``tab`` key contains the tab information, such as the icon & label.

The ``layout`` key tells how the form looks like and where fields are displayed inside it.

The ``fields`` key contains the fields definition, including:

* a label ;
* how the field is displayed: a native HTML ``<input>`` or a custom widget (like date picker or wysiwyg) ;
* validation rules.

Jump to
-------

* :ref:`crud_controller_messages`
* :ref:`crud_controller_tab`
* :ref:`crud_controller_layout`
	* :ref:`crud_controller_form_layout_standard`
	* :ref:`crud_controller_form_expander`
	* :ref:`crud_controller_form_fields`
	* :ref:`crud_controller_form_accordion`
* :ref:`crud_controller_fields`


.. _crud_controller_messages:

Messages list
-------------

This list may not be up to date. Be sure to check the config file `included in the sample application <https://github.com/novius-os/noviusos_monkey/blob/HEAD/config/controller/admin/monkey.config.php>`_ for the latest version.

.. code-block:: php

	<?php
	return array(
		'messages' => array(
			'successfully added' => __('Monkey successfully added.'),
			'successfully saved' => __('Monkey successfully saved.'),
			'successfully deleted' => __('The monkey has successfully been deleted!'),
			'you are about to delete, confim' => __('You are about to delete the monkey <span style="font-weight: bold;">":title"</span>. Are you sure you want to continue?'),
			'you are about to delete' => __('You are about to delete the monkey <span style="font-weight: bold;">":title"</span>.'),
			'exists in multiple lang' => __('This monkey exists in <strong>{count} languages</strong>.'),
			'delete in the following languages' => __('Delete this monkey in the following languages:'),
			'item deleted' => __('This monkey has been deleted.'),
			'not found' => __('Monkey not found'),
			'error added in lang' => __('This monkey cannot be added {lang}.'),
			'item inexistent in lang yet' => __('This monkey has not been added in {lang} yet.'),
			'add a item in lang' => __('Add a new monkey in {lang}'),
		)
	);



.. _crud_controller_tab:

Tab information
---------------

.. code-block:: php

	<?php
	return array(
		'tab' => array(
			'iconUrl' => 'static/apps/noviusos_monkey/img/16/monkey.png',
			// Add form will user 'insert'
			// Edit form will use item's title
			// Translate form (multilingual) will use 'blank_slate'
			'labels' => array(
				'insert' => __('Add a monkey'),
				'blankSlate' => __('Translate a monkey'),
			),
		),
	);


.. _crud_controller_layout:

Layout
------

The layout list all views needed to render the form. The generic format for using a layout is the following:

.. code-block:: php

	<?php
	return array(
		'layout' => array(
			'view_1' => array(
				'view' => 'nos::form/layout_standard',
				'params' => array(
					// View params. Depends on the view.
				),
			),
			// More views can be used here.
		),
	);


In addition to view specific params, Novius OS always include the following vars:

* ``$item`` : the instance of the model currently edited (or added / translated).
* ``$fieldset`` : the form instance which holds all fields definition.


Configuration shortcut
^^^^^^^^^^^^^^^^^^^^^^

Because 95% of the time, we want to use ``nos::form/layout_standard`` as view for the layout, a shortcut was created for simplicity: only write the view  ``params`` of the standard layout.

.. code-block:: php

	<?php
	// The following...
	return array(
		'layout' => array(
			'view_1' => array(
				'view' => 'nos::form/layout_standard',
				'params' => array(
					// View params. Depends on the view.
				),
			),
			// More views
		),
	);


	// ... is the same as this:
	return array(
		'layout' => array(
			// View params for ``nos::form/layout_standard``.
		),
	);


It's much more limitating because you can only use one view to render the layout, and it has to be ``nos::form/layout_standard``. But that's what should be used 95% of the time.



List of natives views included
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Used as container for other layouts / views
"""""""""""""""""""""""""""""""""""""""""""

* ``nos::form/layout_standard``: used as a container for other views ;
* ``nos::form/expander``: used inside ``layout_standard.content`` in the Pages application ;

Used as final view
""""""""""""""""""

* ``nos::form/fields``: used inside ``layout_standard.content`` in the User application ;
* ``nos::form/accordion``: used inside ``layout_standard.menu`` in the Pages application.


.. _crud_controller_form_layout_standard:

1. Layout standard
""""""""""""""""""

This configuration lists the available params for the ``nos::form/layout_standard`` view.

.. code-block:: php

	<?php
	array(
		'view' => 'nos::form/layout_standard',
		'params' => array(
			'title' => 'monk_name',
			'medias' => array('medias->thumbnail->medil_media_id'),
			'large' => true,
			'subtitle' => array('monk_species_id'),
			'content' => array(
				// array of sub-layouts
			),
			'menu' => array(
			),
			'save' => 'save',
		)
	);


.. _crud_controller_form_expander:

2. Expander
"""""""""""

This configuration lists the available params for the ``nos::form/expander`` view.

.. code-block:: php

	<?php
	array(
		'view' => 'nos::form/expander',
		'params' => array(
			'title'   => __('Title'),
			'options' => array(
				'allowExpand' => false,
			),
			'content' => array(
				// array of sub-layouts
			),
		),
	);


.. _crud_controller_form_fields:

3. Fields list
""""""""""""""

This configuration lists the available params for the ``nos::form/fields`` view.

.. code-block:: php

	<?php
	array(
		'view' => 'nos::form/fields',
		'params' => array(
			'fields' => array(
				'monk_summary',
				'wysiwygs->content->wysiwyg_text',
			),
			// 'callback' is optional. Default action is shown here
			'callback' => function($field) {
				echo $field->build();
			},
			// 'begin' is optional. Default is shown here
			'begin' => '<table class="fieldset">',
			// 'end' is optional. Default is shown here
			'begin' => '</table>',
		),
	);


.. _crud_controller_form_accordion:

4. Accordion
""""""""""""

This configuration lists the available params for the ``nos::form/accordion`` view.

.. code-block:: php

	<?php
	array(
		'view' => 'nos::form/accordion',
		'params' => array(
			'accordions' => array(
				'section_1' => array(
					'title' => __('Section title'),
					'fields' => array('page_parent_id', 'page_menu', 'page_menu_title'),
				),
				// More sections
			),
		),
	);



.. _crud_controller_fields:

Fields
------

The documentation for this section is the same as in the :doc:`form generation <form_generation>` page.
