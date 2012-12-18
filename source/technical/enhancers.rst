Enhancers
=========

``Enhancers`` are the front side of an application. They have to be added to pages in order to be visible in your website. It is possible to define parameters for customizing the display of an enhancer.

For instance, the blog application have one enhancer available. Lets say you want to display your blog in the home page of your website:

* You edit your home page ;
* On the wysiwyg menu, click "Application", then choose "Blog" ;
* A configuration popup appears (where you can define the number of items you want to display and the category you wish to filter) ;
* Hit "Save" : a preview appears in the wysiwyg ;
* If you visualise the home page on front, the list of existing blog items appears, customised by the parameters you choose earlier.

This documentation will get onto several aspect of the enhancer:

* :ref:`enhancers_metadata`
* :ref:`enhancers_popup` (number of item per pages / which categories will be displayed)
* :ref:`enhancers_preview`
* :ref:`enhancers_front`
* :ref:`enhancers_url-enhancer`



.. _enhancers_metadata:

1. Define the enhancer in the metadata file
-------------------------------------------

In order to use enhancers you first have to edit the application metadata configuration. All enhancers are defined in the key ``enhancers``.

See below as an example the ``enhancer`` key of the metadata Monkey application.

.. code-block:: php

	array(
		'enhancers' => array(
			'noviusos_monkey' => array(
				'title' => 'Monkey',
				'desc'  => '',
				'iconUrl' => 'static/apps/noviusos_monkey/img/16/monkey.png',
				//'enhancer' => 'noviusos_monkey/front',
				'urlEnhancer' => 'noviusos_monkey/front/main',
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


Each enhancer is represented by a key => configuration pattern. The key is mandatory, configuration must be an array.

Configuration defines main informations about the enhancer:

* ``title``: title of the enhancer displayed when opening the Application menu on the page edition wysiwyg.
* ``desc``: description of the enhancer
* ``iconUrl``: location of the icon displayed when opening the Application menu on the page edition wysiwyg.
* ``enhancer`` or ``urlEnhancer``: url of the application enhancer when displaying it on front. Only one of the two keys can be used : use ``enhancer`` if you don't want to use url enhancers, ``urlEnhancer`` otherwise.
* ``previewUrl``: url of the enhancer preview in the wysiwyg.
* ``dialog``: parameters of the configuration dialog box. Parameters are similar as the ones defined for dialogs in the javascript API.



.. _enhancers_popup:

2. [Back-office] Create a configuration popup
---------------------------------------------

The content of the back-end configuration popup is defined by the ``dialog`` key. A request is made to ``contentUrl`` and the content returned is displayed in the configuration popup. In the case of the Monkey application, the popup content loads ``admin/noviusos_monkey/application/popup`` via ajax.

The popup content is an html form. It is recommended to use the following template for the html form (inspired by the Monkey application) :

.. code-block:: html

	<div id="<?= $id = uniqid('temp_') ?>">
		<form method="POST" action="admin/noviusos_monkey/application/save">
			<!-- content -->
		</form>
	</div>

	<script type="text/javascript">
	require([
		'jquery-nos'
		], function($) {
			$(function() {
				var div = $('#<?= $id ?>')
					.find('a[data-id=close]')
					.click(function(e) {
						div.closest('.ui-dialog-content').wijdialog('close');
						e.preventDefault();
					})
					.end()
					.find('form')
					.submit(function() {
						var self = this;
						$(self).ajaxSubmit({
							dataType: 'json',
							success: function(json) {
								div.closest('.ui-dialog-content').trigger('save.enhancer', json);
							},
							error: function(error) {
								$.nosNotify('An error occured', 'error');
							}
						});

						return false;
					})
					.nosFormUI();
			});
		});
	</script>


Please note that the form is submited via ajax. The returned content must be a json formatted array with two keys:

* ``config`` which is the configuration of the enhancer
* ``preview`` which is an HTML preview of the enhancer

You must then trigger the ``save.enhancer`` event on the popup content in order to close the popup and display the newly created enhancer in the wysiwyg.

``div.closest('.ui-dialog-content').trigger('save.enhancer', json);``


.. _enhancers_preview:

3. [Back-office] Display a preview in the Wysiwyg
-------------------------------------------------

The enhancer is displayed on the wysiwyg by loading via ajax the content of ``previewUrl``. In the case of the Monkey application, it will display the content returned by ``admin/noviusos_monkey/application/preview``.


.. _enhancers_front:

4. [Front-office] Display your content on the website
-----------------------------------------------------

Once the wysiwyg is saved and the page published, the enhancer will be available on Front. The content displayed comes from ``enhancer`` or ``urlEnhancer``.

In the case of the Monkey application, the diplayed content is returned from ``noviusos_monkey/front/main`` hence the ``main`` action of the ``Controller_Front`` of the Monkey application.

The action called takes in parameter the configuration of the enhancer (which was set in the configuration popup).


.. _enhancers_url-enhancer:

5. URL enhancers
----------------

If the key ``urlEnhancer`` was populated, this will allow the enhancer to manage more complex urls.

Lets say your enhancer is located on "page.html" ; it will then be able to manage urls like "page/1.html", "page/more.html" or "page/more/1.html".

Like in the previous case, the content is still returned by the ``main`` action, but it is possible to get the extended url by calling the function ``$this->main_controller->getEnhancerUrl();`` and customize the content.

The controller must in this case implement a ``get_url_model()`` static function. See below the one in the Monkey application:

.. code-block:: php

	<?php
	public static function get_url_model($item, $params = array())
	{
		$model = get_class($item);
		$page = isset($params['page']) ? $params['page'] : 1;

		switch ($model) {
		case 'Nos\Monkey\Model_Monkey' :
			return urlencode($item->monk_virtual_name).'.html';
			break;

		case 'Nos\Monkey\Model_Species' :
			return 'species/'.urlencode($item->mksp_virtual_name).($page > 1 ? '/'.$page : '').'.html';
			break;
		}

		return false;
	}


This function is related with the Urlenhancer behaviour implemented in the objects (see :doc:`behaviours`). Indeed, ``urls($params = array())``, ``url_canonical($params = array())`` and ``url($params = array())`` indirectly call the controller static function ``get_url_model($item, $params = array())``.

Lets say the monkey enhancer is located on both "first-page.html" and "page-2.html", and we loaded a monkey object ``$monkey``. If we call ``$monkey->urls();``, the Urlenhancer behaviour will iterate on each published page (of the same language if the object has the translatable behaviour) and complete the URL by calling the ``get_url_model()`` function (the ``$item`` parameter will then be ``$monkey``, and the ``$params`` parameter will be the ``$params`` parameter of the ``urls($params = array())`` function).

In our case the returned array of ``$monkey->urls();`` will be similar to:

.. code-block:: php

	<?php
	array(
	  '1::monkey-name.html' =>  'first-page/monkey-name.html',
	  '2::monkey-name.html' => 'page-2/monkey-name.html'
	);

