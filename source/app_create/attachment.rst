Add attachment
##############

Principles
**********

The class :ref:`api:php/classes/attachment` allows you to manage attachments.

Attachments are saved in the :file:`local/data/files/` directory.

Usually attachments are joined to a :ref:`api:php/models/model` but it is possible to manage them as you wish.
You just need a tiny configuration in order to define an :ref:`api:php/classes/attachment`.

.. code-block:: php
	:emphasize-lines: 3,4

	<?php

	$attachment = \Nos\Attachment::forge('my_id', array(
		'dir' => 'apps'.DS.'myapps',
	));

On above example, attachment will be saved in the :file:`local/data/files/apps/myapps/my_id/` directory.

In order to save a file, you will just need:

.. code-block:: php

	$attachment->set($_FILES['file']['tmp_name'],  $_FILES['file']['name']);
	$attachment->save();

On above example, we save an uploaded file as an attachment.
File location will be: :file:`local/data/files/apps/myapps/my_id/original_name.ext`
where ``original_name.ext`` is the original name of the uploaded file collected from ``$_FILES['file']['name']``.


Joined to a model
*****************

In the case a file is joined to a :ref:`api:php/models/model`, it is even simpler.
In your class, you just need to write:

.. code-block:: php

	class Model_Example extends \Nos\Orm\Model
	{

		protected static $_attachment = array(
			'avatar' => array(),
			'document' => array(),
		);

This way, each ``Model_Example`` item will have two attachment: ``avatar`` and ``document``.

.. code-block:: php

	$item = Model_Example::find('first');
	$item->avatar->set($_FILES['file']['tmp_name'], $_FILES['file']['name']);
	$item->avatar->save();

Details
*******

For more details, check :ref:`api:php/classes/attachment`.

Extensions
==========

When you create a new :ref:`api:php/classes/attachment`, you can specify a list of authorized extensions by adding the
``extensions`` key to the configuration. Value should be an array of authorized extensions.

If your file has to be an image, you can set the special key ``image`` to true.

URL alias
=========

By default, your attachment will be available in this URL:

:file:`http://www.domain.com/data/files/{dir}/{id}/{file_name}.{extension}`

If ``dir`` value is, as often, ``apps/my_app/my_file_type/``, the url can be quite extensive.

Define an ``alias`` class in your :ref:`api:php/classes/attachment` configuration.
``alias`` value will replace ``dir`` value in URL.


Secured attachment
==================

It is possible to secure your attachments, in order to limit access only for authenticated user for example.
You just need to define on configuration the ``check`` key which value is a `fonction de callback <http://php.net/manual/fr/language.types.callable.php>`_.
Each time file is requested, the system will execute this function, with first parameter the current
:ref:`api:php/classes/attachment` instance, in order to check the file is available in this context.

Example:

.. code-block:: php

	class Verification
	{
		public static function check($attachment)
		{
			return isset($_SESSION['user_connected']) && $_SESSION['user_connected'];
		}
	}

	$attachment = \Nos\Attachment::forge('my_id', array(
		'dir' => 'apps'.DS.'myapps',
		'check' => array('Verification', 'check'),
	));

In the upper example, if the user is logged (session key ``user_connected`` set to true), the file will be available.
If not, the url will throw a 404 error.