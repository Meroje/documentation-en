Sharing
*******

.. seealso::

	`Sharing in Novius OS <http://novius-os.github.com/docs/applications.html#sharing>`_


.. _sharing_content-nuggets:

Content nuggets
===============

A content nuggets consists of a set of data to be shared.

Data structure
--------------

The content nugget's data have a standard structure:

* Title
* URL
* Text
* Image

In order to share an item, it is possible to add to its class the :ref:`Sharable Behaviour <api:php/behaviours/sharable>`, which
will define how to fill these data.


.. _sharing_data-catchers:

Data catchers
=============

Data catchers are components which use content nuggets.

Data catchers are defined by applications in the :file:`metadata.config.php` file, as are templates, enhancers or
launchers.

Included in Novius OS
---------------------

Triggered by the user
^^^^^^^^^^^^^^^^^^^^^

* Twitter
* Facebook
* Google+
* Blog

The ``Blog`` data catcher can be used in order to create blog posts from other items, as monkeys (our sample
application) or books.


Example : **Twitter**
---------------------

Here is how the Twitter data catcher is defined:

.. code-block:: php

	<?php

	return array(
        'data_catchers' => array(
            'noviusos_simpletwitter' => array(
                'title' => 'Twitter',
                'description'  => '',
                'iconUrl' => 'static/apps/noviusos_simpletwitter/img/twitter.png',
                'action' => array(
                    'action' => 'window.open',
                    'url' => 'https://twitter.com/intent/tweet?text={{urlencode:'.\Nos\DataCatcher::TYPE_TITLE.'}}&url={{urlencode:absolute_url}}',
                ),
                'onDemand' => true,
                'specified_models' => false,
                'required_data' => array(
                    \Nos\DataCatcher::TYPE_TITLE,
                ),
                'optional_data' => array(
                    \Nos\DataCatcher::TYPE_URL,
                ),
            ),
        ),
	);

The **Twitter** data catcher require the content nuggets' title. URL is optional (but will be used if provided).
