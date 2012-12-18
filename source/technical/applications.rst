Applications
============

Applications are the easiest way to add functionalities to Novius OS. A few applications are available : Blog, News stories, Slideshows... and `Monkeys <https://github.com/novius-os/noviusos_monkey>`_, a sample application.

The application management system has been implemented so that is is really easy to add and extend applications.

* :ref:`applications_files-organisation`
* :ref:`applications_metadata-configuration-file`
* :ref:`applications_appdesk`

.. _applications_files-organisation:

Files organisation
------------------

The files are organised the same way as application in FuelPHP.

.. image:: /technical/files_organisation.png

.. _applications_metadata-configuration-file:

Metadata configuration file
---------------------------

The metadata configuration file is mandatory. It indicates to the core the key information it needs to use the application. Below is the metadata file of the Monkeys application:

.. code-block:: php

	<?php
	return array(
		'name'    => 'Monkey : Novius OS Application Bootstrap',
		'version' => '0.1',
		'icon16' => 'static/apps/noviusos_monkey/img/16/monkey.png',
		'provider' => array(
			'name' => 'Provider',
		),
		'namespace' => 'Nos\Monkey',
		'launchers' => array(
			'provider_launcher' => array(
				'name'    => 'Monkey',
				'url' => 'admin/noviusos_monkey/appdesk',
				'iconUrl' => 'static/apps/noviusos_monkey/img/32/monkey.png',
				'icon64'  => 'static/apps/noviusos_monkey/img/64/monkey.png',
			),
		),
		'enhancers' => array(
			'noviusos_monkey' => array(
				'title' => 'Monkey',
				'desc'  => '',
				//'enhancer' => 'noviusos_monkey/front',
				'urlEnhancer' => 'noviusos_monkey/front/main',
				'iconUrl' => 'static/apps/noviusos_monkey/img/16/monkey.png',
				'previewUrl' => 'admin/noviusos_monkey/application/preview',
				'dialog' => array(
					'contentUrl' => 'admin/noviusos_monkey/application/popup',
					'width' => 450,
					'height' => 200,
					'ajax' => true,
				),
			),
		),
	);

The metadata configuration file returns a key => value array:

* ``name``: name of your application
* ``version``
* ``icon16``: 16x16 icon representing your application (used for tabs)
* ``provider``: key => value array giving informations about the provider
* ``namespace``: namespace of all classes in your application
* ``launchers``: key => value array providing informations about launchers (icons at desktop which opens your application in a new tab)

  The ``key`` is the launchers identifier.
  
  Value is key => value array.
 
	* ``name``
	* ``url``: url called when opening your application
	* ``iconUrl``: 32x32 icon for the tab that open your application
	* ``icon64``: 64x64 icon for the desktop
    
* ``enhancers``: see :doc:`enhancers`
* ``data-catchers``: see :doc:`sharing`
* ``template``: key => value array giving registered templates

	.. code-block:: php

		<?php
			'templates' => array(
				'top_menu' => array(
					'file' => 'noviusos_templates_basic::top_menu',
					'title' => 'Default template with a top menu',
					'cols' => 1,
					'rows' => 1,
					'layout' => array(
						'content' => '0,0,1,1',
					),
				),
				// ...
			),

Each templates can be seperated in different parts. You can have a standard template where all is displayed in one location, but you can also have more complexe templates with a left and a right side for example. The idea is to provide this information to Novius OS in order to allow the core to manage multiple wysiwygs. Wysiwygs are displayed in a grid: you are able to choose position and scale of their wysiwygs.

	* ``file``: location of the template
	* ``title``: title of the template, will be used when you create / edit a page and choose a template for it.
	* ``cols``: number of columns in the grid
	* ``rows``: number of rows in the grid
	* ``layout``: layout of the wysiwygs in the grid. key => value array

		* the key is the template identifier
		* the value is a string, representing the left, top position, and width, height, comma seperated

.. _applications_appdesk:

App Desk
--------

The App Desk allows you to easily list and filter your application data. See :doc:`appdesk`.
