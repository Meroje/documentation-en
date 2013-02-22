Adding a field
##############

We'll start from an example to explain how it works.

Let's add a ``Source`` field on blog posts, to allow us to fill in an external URL from where the original content was produced.



In the database
***************

.. code-block:: sql

    ALTER TABLE `nos_blog_post` ADD `post_source` VARCHAR(255);


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

