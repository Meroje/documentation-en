Translate an application
########################

Each application possess the :file:`lang` directory where are located translation files, that will be called
**dictionaries**.

This directory include as many subdirectories as languages we want to translate the application into. Dictionaries will
be define in these subdirectories.

These language directories are named following locales, as for example :file:`fr` (French), or :file:`en` (English).

Dictionaries are PHP files returning a array, similarly as configuration file.

.. seealso:: :ref:`I18n class API <api:php/classes/i18n>`


File :file:`metadata.config.php`
================================

``metadata`` are a particular case, since they are cached. They need a translation file on their own. First step is to
define which dictionaries ``metadata`` need to be translated.

.. code-block:: php
   :emphasize-lines: 6

    <?php

    return array(
        'name' => 'My app',
        'namespace' => 'My\App',
        'i18n_file' => 'my_app::metadata',
        // ... other keys
    );

As all change on :file:`metadata`, don't forget to apply changes in the application manager.

Next, you need to create the :file:`my_app::lang/fr/metadata.lang.php` dictionary:

.. code-block:: php

    <?php

    return array(
        'My app' => 'My application',
    );

Novius OS automatically knows which keys has to be translated in the :file:`metadata` file and will get corresponding
translations.


Other files
===========


Elsewhere, you need to do the following in order to translate files (including PHP):

1. Use the :func:`__` function in order to get translations;
2. For each file, configure in which dictionary will be used translations.


It is quite simple for view and configuration files:

.. code-block:: php

    <?php

    // Configure the __() function
    Nos\I18n::current_dictionary('my_app::common');

    __('Translate this'); // Translation will be collected from my_app::lang/<lang>/common.lang.php


It is a little more complicated for admin controllers, because language depends on the user and is known only after
authentication, which happens in ``before()``.

``prepare_i18n()`` has been implemented to solve this problem:


.. code-block:: php
   :emphasize-lines: 9-12

    <?php

    namespace Nos\Form;

    class Controller_Admin_Form extends \Nos\Controller_Admin_Crud
    {
        public function prepare_i18n()
        {
            // Configure language file depending on user
            parent::prepare_i18n();
            // Configure the __() function
            \Nos\I18n::current_dictionary('noviusos_form::common');
        }

        // Other methods using __()
    }

It is possible to use many dictionaries in only one file ; just use an array instead of a string. Translation will be
choose from the first file containing required key.


.. code-block:: php
   :emphasize-lines: 3

    <?php

    Nos\I18n::current_dictionary(array('my_app::dictionary', 'my_app::common'));

    // Translation will be collected from my_app::lang/<lang>/dictionary.lang.php if it exists
    // Otherwise in my_app::lang/<lang>/common.lang.php
    __('Translate this');