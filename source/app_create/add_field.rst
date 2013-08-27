Add fields
##########


.. seealso:: /app_extend/add_field

Most fields added need a column in the model associated MYSQL table.

Fields are then added in the CRUD form using the ``fields`` key in the configuration file.

Syntax used is using a existing feature, which defines how a column displays.

.. seealso::

    `FuelPHP documentation about model properties <http://docs.fuelphp.com/packages/orm/creating_models.html#propperties>`__

Moreover, Novius OS team implemented :ref:`renderers <api:php/renderers>`, which allows more freedom. Some renderer allow to
select medias, pages, date.

Configuration example:

.. code-block:: php

	<?php
	return array(
		'name' => array(
			'label' => 'Text displayed next to field',
			'form' => array(
				'type' => 'text',
				'value' => 'Default field',
			),
			'validation' => array(),
	);


Standards fields
----------------

Bold text is the ``type`` property value:

* <input type="**text**">
* <input type="**password**">
* <**textarea**>
* <**select**>
* <input type="**radio**">
* <input type="**checkbox**">
* <input type="**submit**">
* <input type="**button**">
* <input type="**file**">

<select> field
^^^^^^^^^^^^^^

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
				),
			),
			'validation' => array('required'),
		),
	);


<button type="submit">
^^^^^^^^^^^^^^^^^^^^^^

* ``type = submit`` generate ``<input type="submit">``
* ``type = button`` generate ``<input type="button">``

``tag`` property can be used to force HTML tab, for the ``submit`` button case.

FuelPHP use automatically ``value`` as button text.

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


Renderers (enhanced fields)
---------------------------

``renderers`` list is available in :ref:`API documentation <api:php/renderers>`.

