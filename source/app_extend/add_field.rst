Adding a field
##############

We'll start from an example to explain how it works.

Let's add a ``Source`` field on blog posts, to allow us to fill in an external URL from where the original content was produced.



In the database
***************

.. code-block:: sql

    ALTER TABLE `nos_blog_post` ADD `post_source` VARCHAR(255);

In the model
************

2 choices:

* Declare the new field in the model ``properties``.
* Activate the cache mechanism of models ``properties``.

Declare the field
=================

We're going to listen the event on the model config file.

.. code-block:: php

    <?php

    Event::register_function('config|noviusos_blog::model/post', function(&$config) {
        $config['properties']['post_source'] = array(
            'default' => null,
            'data_type' => 'varchar',
            'null' => false,
        );
    });

.. seealso::

    `Defining properties in FuelPHP documentation <http://fuelphp.com/docs/packages/orm/creating_models.html#/propperties>`__

Activate the ``properties`` cache
=================================

* Create the file :file:`local/config/config.php` by copying :file:`local/config/config.php.sample` (if necessary).
* Uncomment the line (or create it) with the key ``cache_model_properties`` and set it to ``true``:

    .. code-block:: php

        <?php

        return array(
            //...

            'novius-os' => array(
                //...
                'cache_model_properties' => true,

                //...
            ),
        );

When activated, all models ``properties`` will be cached in the directory :file:`local/cache/fuelphp/model_properties/`.
When a column is added and not declared, the first call to ``get()`` or ``set()`` for this column will fetch the schema
from the DB and update the cached ``properties`` .

.. warning::

    This mechanism `only works with the MySQL and MySQLi drivers <http://fuelphp.com/docs/packages/orm/creating_models.html#/creation>`__.

.. seealso::

    :ref:`Documention for Novius OS configuration <api:php/configuration/software>`.


In the form
***********

The addition / edition form of a blog post is defined in its CRUD configuration. To extend it, we'll use an event!

In the :file:`local/bootstrap.php` file (create it if necessary):

.. code-block:: php

    <?php

    Event::register_function('config|noviusos_blog::controller/admin/post', function(&$config) {

        // Add a 'post_source' field (type 'text')
        $config['fields']['post_source'] = array(
            'label' => 'Source originale :',
            'form' => array(
                'type' => 'text',
                'placeholder' => 'http://',
            ),
        );

        // Display the field inside the form
        // We create a new 'Source' expander in the right menu
        $config['layout']['menu']['Source'] = array('post_source');
    });


The form now contains an additional editable field, as you can see below:

.. image:: images/blog_source_field.png
	:alt: 'source' field inside the blog post form
	:align: center


In the visualisation
********************

For the view, let's create the :file:`local/views/apps/noviusos_blognews/front/post/content.view.php` file.

.. code-block:: html+php

    <?php

    // Let's include the original file (it displays the content)
    include APPPATH.'/applications/noviusos_blognews/views/front/post/content.view.php';

    // And add the 'source' field right after
    if (!empty($item->post_source)) {
        ?>
        <p class="blognews_source">
            <?= __('Source:') ?>
            <a href="<?= htmlspecialchars($item->post_source) ?>">
                <?= htmlspecialchars($item->post_source) ?>
            </a>
        </p>
        <?php
    }

