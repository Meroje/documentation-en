Form generation and renderers
=============================

**Please note:** *This page is entitled "renderers", but it currently makes reference to Widgets. We're going to rename the latter in a future version of Novius OS. We thought it was easier to use the final name for the title of this page.*

Novius OS can generate forms using on a configuration array.

The syntax is based on an existing FuelPHP feature, which defines how a column is displayed in the `properties <http://docs.fuelphp.com/packages/orm/creating_models.html#propperties>`_ of a model.

In addition to standard form fields, Novius OS can also create widgets (media, date, page selector, ...).

Table of contents
-----------------

* :ref:`form_generation_example`
* :ref:`form_generation_inluded_in_fuelphp`
* :ref:`form_generation_widgets_noviusos`

    * :ref:`form_generation_widget_media`
    * :ref:`form_generation_widget_date`
    * :ref:`form_generation_widget_page`


.. _form_generation_example:

Example of configuration file
------------------------------

.. code-block:: php

	<?php
	return array(
		'name' => array(
			'label' => 'Text displayed next to the field',
			'form' => array(
				'type' => 'text',
				'value' => 'Default value',
			),
			'validation' => array(),
	);


.. _form_generation_inluded_in_fuelphp:

Form fields included in FuelPHP
--------------------------------

The bold text is the value of the ``type`` property.

* <input type="**text**">
* <input type="**password**">
* <**textarea**>
* <**select**>
* <input type="**radio**">
* <input type="**checkbox**">
* <input type="**submit**">
* <input type="**button**">
* <input type="**file**">

.. code-block:: php

	<?php
	return array(
		'gender' => array(
			'label' => 'Gender',
			'form' => array(
				'type' => 'select',
				'options' => array(
					'm' => 'Male',
					'f' => 'Female',
				)
			),
			'validation' => array('required'),
		),
	);


<button type="submit">
^^^^^^^^^^^^^^^^^^^^^^

* ``type = submit`` generates ``<input type="submit">``
* ``type = button`` generates ``<input type="button">``

The ``tag`` property can be used to enforce the HTML tag, so we can generate ``<button type="submit">``.
FuelPHP will automatically use the value as text when rendering a ``<button>`` 

.. code-block:: php

	<?php
	return array(
		'save' => array(
			'form' => array(
				'type' => 'submit',
				'tag' => 'button',
				'value' => 'Save',
			),
		),
	);



.. _form_generation_widgets_noviusos:

Widgets provided by Novius OS
------------------------------

.. _form_generation_widget_media:

Media picker
^^^^^^^^^^^^

This widget is used to pick a file from the media centre. The UI part uses the jQuery UI widget called ``input-file-thumb``.
The field contains the value of a media_id.

Widget options
""""""""""""""

* ``mode`` : Possible values are ``all`` (default) or ``image``
* ``inputFileThumb``: options for the inputFileThumb widget used to render the UI. See the `documentation <http://www.novius-labs.com/contributions/jquery-plugin-inputfile/documentation.html>`_ of the plugin for all available options.

The ``inputFileThumb.file`` key will automatically be populated with the URL of the media if a ``value`` is provided in the widget.

Example
"""""""

.. code-block:: php

	<?php
	return array(
		'label' => '',
		'form' => array(
			'value' => 2, // ID of the previously selected media
		),
		'widget' => 'Nos\Widget_Media',
		'widget_options' => array(
			'mode' => 'image',
			'inputFileThumb' => array(
				'title' => 'Title of the image',
			),
		),
	);


.. _form_generation_widget_date:

Date picker
^^^^^^^^^^^

This widget uses the jQuery UI widget called ``datepicker``.

Widget options
""""""""""""""

* ``datepicker``: options for the datepicker widget used to render the UI. See the `documentation <http://jqueryui.com/demos/datepicker/>`_ of the plugin for all available options.
* ``wrapper`` : HTML string to wrap the ``<input>`` + the generated image to open the datepicker.

Default values
""""""""""""""

.. code-block:: php

	<?php
	array(
		'datepicker' => array(
			'showOn' => 'both',
			'buttonImage' => 'static/novius-os/admin/novius-os/img/icons/date-picker.png',
			'buttonImageOnly' => true,
			'autoSize' => true,
			'dateFormat' => 'yy-mm-dd',
			'showButtonPanel' => true,
			'changeMonth' => true,
			'changeYear' => true,
			'showOtherMonths' => true,
			'selectOtherMonths' => true,
			'gotoCurrent' => true,
			'firstDay' => 1,
			'showAnim' => 'slideDown',
		),
		'wrapper' => '', //'<div class="datepicker-wrapper"></div>',
	);


Example
"""""""

.. code-block:: php

	<?php
	return array(
		'label' => '',
		'form' => array(
			'value' => '2012-06-25', // value & datepicker.dateFormat must be coherent
		),
		'widget' => 'Nos\Widget_Date_Picker',
		'widget_options' => array(
			'datepicker' => array(
				'dateFormat' => 'yy-mm-dd',
			),
		),
	);



.. _form_generation_widget_page:

Page selector
^^^^^^^^^^^^^

Used to select ONE page from the CMS. It displays a tree containing the pages and radio buttons.

Widget options
""""""""""""""

* ``lang``: A valid locale listed in the config file. For example, ``en_GB`` or ``fr_FR`` are valid values
* ``height`` : Default to ``150px``
* ``width`` : Default to ``null`` (means 100% of the container)


Example
"""""""

.. code-block:: php

	<?php
	return array(
		'label' => '',
		'form' => array(
			'value' => 2, // ID of the page. Must be in the appropriate lang for the pre-selection to work.
		),
		'widget' => 'Nos\Widget_Page_Selector',
		'widget_options' => array(
			'lang' => 'en_US',
		),
	);


