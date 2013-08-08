Content sharing
***************

.. seealso::

	`‘Content sharing in Novius OS’ infographic <http://novius-os.github.com/docs/applications.html#sharing>`_


.. _sharing_content-nuggets:

Content nuggets
===============

A content nugget is a coherent set of data meant to be shared.

Data structure
--------------

Content nuggets’ data are of the following nature:

* Title
* URL
* Text
* Image

For an application to share its content, it must have the :ref:`Sharable Behaviour <api:php/behaviours/sharable>`. This entails defining the content nugget’s structure for the application—i.e. which fields are to be shared.


.. _sharing_data-catchers:

Data catchers
=============

Data catchers are application components which make use of the content nuggets.

They are defined in the application’s :file:`metadata.config.php` file, as are other components (templates, enhancers and
launchers).

Data catchers included in Novius OS
-----------------------------------

* Simple Twitter
* Simple Facebook
* Simple Google+
* Blog

The ``Blog`` data catcher is used to create a new blog post with other applications’ content, especially customer-specific applications. Let’s say you add a new product to your catalogue, this data catcher allows you to easily announce this new addition on your blog.


Example : Simple Twitter
------------------------

Here is how the data catcher for simple sharing to Twitter is defined:

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

The **Simple Twitter** data catcher requires content nuggets featuring at least a title. URLs are optional but are used when provided.
