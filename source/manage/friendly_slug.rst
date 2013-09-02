Friendly slug
#############

All segments of URLs builded in Novius OS are cleaned by the friendly slug mechanism.

By default:

* all characters ``?``, ``:``, ``\``, ``/``, ``#``, ``[``, ``]``, ``@``, ``&`` and space are replaced by ``-``.
* transform to lower case.
* remove trailing ``-``.
* replace multiple ``-`` by one.

But you can use others rules or define your own rule.
You can also have special rules for :doc:`contexts </understand/multi_context/principles>`.

Four setups of rules are defined:

* ``default`` setup (like describe above)
* ``no_accent`` setup. All accent characters are replaced by the equivalent character without accent.
* ``no_special`` setup. All characters that are not a word character, a ``-`` or a ``_`` are replaced by ``-``.
* ``no_accent_and_special`` setup. Combination of ``no_accent`` and ``no_special`` setups.

A sample configuration file is available in :file:`local/config/friendly_slug.config.php.sample`.
Just rename (or copy) it to :file:`local/config/friendly_slug.config.php`, and update it to your case.

Default setup
=============

To change the default setup of rules:

* add a key to ``setups``.
* Set ``active_setup`` to this new key.

.. code-block:: php

    <?php
    return array(
        'active_setup' => 'my_default',

        'setups' => array(
            'my_default' => array(
                // Use the 'no_accent' setup
                'no_accent',

                // Replace space by '_'
                ' ' => '_',

                 // All characters that are not a word character, a '-' or a '_' or a '*' are replaced by '-'.
                '[^\w\*\-_]' => array('replacement' => '-', 'flags' => 'i'),
            ),
        ),
    );

Setup for context
=================

To define specific rules to context, define a new key equal to the context ID in ``setups`` array.

.. code-block:: php

    <?php
    return array(
        'setups' => array(
            'main::en_GB' => array(
                //... Set here your specific rules for context main::en_GB
            ),
        ),
    );


.. seealso::

	:ref:`Friendly slug API <api:php/configuration/friendly_slug>`.