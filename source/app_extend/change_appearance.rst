Changing the appearance on the website
######################################


We'll start from an example to explain how it works.

On the `Novius OS <http://www.novius-os.org>`__ website, we personalised how the blog posts are displayed. Here's how it looks like:


Default design of the 'Blog' application :

.. image:: images/blog_display_original.png
	:alt: Default list of the 'Blog' application
	:align: center


Personalised design on the Novius OS.org website (our goal):

.. image:: images/blog_display_custom.png
	:alt: Personalised list of the 'Blog' application
	:align: center


Changing the view
*****************

1\ :sup:`st`\  solution: extending the view
===========================================

Thanks to the cascading file system, we can copy the original :file:`noviusos_blognews::views/front/post/item.view.php`
file in our :file:`local` directory: :file:`local::views/apps/noviusos_blognews/front/post/item.view.php`

.. code-block:: html+php

    <div class="blognews_post blognews_post_item">
        <div class="blognews_primary_information">
            <?= \View::forge('noviusos_blognews::front/post/title', array('item' => $item)) ?>
            <?= \View::forge('noviusos_blognews::front/post/summary', array('item' => $item)) ?>
        </div>
        <div class="blognews_secondary_information">
            <?= \View::forge('noviusos_blognews::front/post/publication_date', array('item' => $item)) ?>
            <?= \View::forge('noviusos_blognews::front/post/tags', array('item' => $item)) ?>
        </div>
    </div>

We deleted the thumbnail, author, categories and comment count from this view file.


2\ :sup:`nd`\  solution: extends the configuration
==================================================

The blog application allows to disable some elements from its configuration. In our situation, it's possible for every
elements we don't want to display, expect the thumbnail.

When using this blog configuration file, it acts on both the list and the full item view, which is not really what we
want (so this solution is just shown as an example).

Thanks to the cascading file system, we can copy the original :file:`noviusos_blognews::config/config.php` file in our
:file:`local` directory: :file:`local::config/apps/noviusos_blognews/config.php`

.. code-block:: php

    <?php

    // We only keep the keys we want to alter
    return array(
        'categories' => array(
            'show' => false,
        ),
        'authors' => array(
            'show' => false,
        ),
        'comments' => array(
            'show' => false,
        ),
    );


Adding the CSS
**************


1\ :sup:`st`\  solution: extending the view
===========================================

We create the :file:`local::views/apps/noviusos_blognews/front/post/list.view.php` file:

.. code-block:: php

    <?php

    // We add our custom CSS file
    \Nos\Nos::main_controller::addCss('static/css/blog_custom.css');

    // We include the original file (which displays the post list)
    include APPPATH.'applications/noviusos_blognews/views/front/post/list.view.php';


Our altered view first include a CSS file (to be created in :file:`public/static/css/blog_custom.css`), then calls the original view.


2\ :sup:`nd`\  solution: directly take action on the template
=============================================================

It's also possible to include the CSS file with the ``front.start`` event, but in this case, it will be included on
every pages of your website, not only on the blog page.

In the :file:`local/bootstrap.php` file (create it if necessary):

.. code-block:: php

    <?php

    // This event is triggered when loading a page of the website
    Event::register('front.start', function() {
        \Nos\Nos::main_controller::addCss('css/blog_custom.css');
    });

For the `Novius OS <http://www.novius-os.org>`__ website, we created our own templates, which are bundled with the
appropriate CSS files to change how the blog is displayed.

