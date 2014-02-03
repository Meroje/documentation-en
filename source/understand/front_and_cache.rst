Front-Office and cache
######################


Novius OS is using Apache **mod_rewrite** (or any equivalent on other servers) to display pages in the front-office.

Every URL ending with ``.html``, the home page and the folders are redirected to the :file:`NOSROOT/public/htdocs/novius-os/front.php` file.

This file loads Novius OS and asks the :ref:`Controller_Front <api:php/classes/controller_front>` to handle the URL.

The :ref:`Controller_Front <api:php/classes/controller_front>` parses the URL and figure out the associated cache file path.
From there, several things can happen.


Cache execution
===============

When the cache file associated to the current URL exists.

The cache is saved in the :file:`NOSROOT/local/cache/page/` directory. First level of the tree is the domain name. Below,
the URL path is used.

The cache file is a PHP file. First lines are responsible to check whether the cache file is still valid or it has expired.
It also has a way to recreate the :ref:`Controller_Front <api:php/classes/controller_front>` properties which were available
when the cache was generated (page instance, URLs, status, headers, custom data).

If it's still valid, the cache is executed and display the page to the user, with the saved ``status`` and ``headers``.


Cache generation
================

When the cache file associated to the current URL doesn't exists or has expired.

The :ref:`Controller_Front <api:php/classes/controller_front>` will find the appropriate page based on the URL. Once the
page is known, it can determine the associated template and the WYSIWYG list to insert into the template.

When WYSIWYG contains enhancer, they are executed and their content is saved.

Then the template (it's a View) is executed with the following data: ``$wysiwyg`` array, the page ``$title``, the ``$page``
and the :ref:`Controller_Front <api:php/classes/controller_front>` (``$main_controller``).

The generated content is saved into the cache file.

Once the cache is generated, it's being executed (see above section) to send the content to the user, with the ``status``
and ``headers`` specified during the cache generation (especially by the enhancers).


Possible interactions
=====================

During the process :ref:`Controller_Front <api:php/classes/controller_front>` triggers several events at key locations.
By using them, it's possible to alter the process.

.. seealso:: :ref:`Front-office events <api:php/events/front-office>`

You also have control on the process by using the :ref:`Controller_Front <api:php/classes/controller_front>` methods. You
can retrieve the front controller instance using :ref:`\Nos\Nos::main_controller() <api:php/classes/nos/main_controller>`,
or just use ``$this->main_controller`` inside an enhancer.

Alter the generated content
---------------------------

In some situations, you may want to generate your own content and skip the page template. For instance, an enhancer can
send a RSS feed. It's done using the ``sendContent()`` method of the :ref:`Controller_Front <api:php/classes/controller_front>`.

Below is an example (enhancer code):

.. code-block:: php

    <?php

    $this->main_controller->setHeader('Content-Type', 'application/xml');
    $this->main_controller->setCacheDuration(60 * 30); // Cache duration is set to 30min
    return $this->main_controller->sendContent($rss); // The $rss variable contains the RSS feed (XML content)

The cache file will only contains the RSS feed content and the HTTP response will contain a header with the correct ``content-type``.

Executing outside the cache
===========================

In some situations, the caching system is too much effective. For instance, if a portion of the template or of the enhancer
should be different depending on whether the user is logged in or not. In this situation, it's useful to tell the cache to
execute a PHP code each time rather than saving its result.

To do so, the :ref:`FrontCache <api:php/classes/frontcache>` provides the ``callHmvcUncached()`` and ``viewForgeUncached`` methods.

.. code-block:: php

    <?php

    // This will execute a controller's action each time the cache is executed.
    \Nos\FrontCache::callHmvcUncached(
        'uri/controller',
        array(
            'id' => \My_User::get_current_user_id() // Just as an example, the My_User class doesn't really exists
        )
    );

    // or

    // This will include the view each time the cache is executed (rather than saving its result)
    \Nos\FrontCache::viewForgeUncached(
        'uri/view', // View path
        array(
            'id' => \My_User::get_current_user_id()
        ),
        false
    );


Suffix Handler
==============

You can configure the cache path to also vary based upon any parameter, in addition to the current URL. For instance,
different GET parameters doesn't change the cache path (the same file is used for the same URL, with or without GET).

To do so, use the ``addCacheSuffixHandler()`` method from the :ref:`Controller_Front <api:php/classes/controller_front>`.

.. code-block:: php

    <?php

    \Nos\Nos::main_controller()->addCacheSuffixHandler(array(
        array(
            'type' => 'GET',
            'keys' => array('my_param'),
        ),
    ));

    // or

    // The callback function must return a string (empty string when you don't want to alter the cache path)
    \Nos\Nos::main_controller()->addCacheSuffixHandler(array(
        array(
            'type' => 'callable',
            'callable' => array('MyClasse', 'myMethod'),
            'args' => array(
                'example arg'
            ),
        ),
    ));

In the first example, the cache system will generate one cache files for each different value of the GET[my_param] variable.

In the second example, the cache system will call the ``MyClasse::myMethod('example arg')`` method, which is responsible
to return a suffix to the file path if necessary.
