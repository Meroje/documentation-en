Migration guide from the Chiba 2 version to the D version
###############################################################

Upgrade your Novius OS and its applications
*******************************************

See :doc:`/install/upgrade` page if you didn't already do it.

Migrate your developments
**************************

Breaking changes
----------------

.. _release/migrate_from_chiba.2_to_d/fuelphp:

FuelPHP from 1.6 to 1.7.1
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Take a look of these three changelog of FuelPHP for backward compatibility notes:

* `FuelPHP 1.6.1 <https://github.com/fuel/fuel/blob/f5c031a32e2e205eec573121d8417360cef4d609/CHANGELOG.md>`__
* `FuelPHP 1.7 <https://github.com/fuel/fuel/blob/1c4e81b3941c833a8dcf0e6565d4bbe68dc65f03/CHANGELOG.md>`__
* `FuelPHP 1.7.1 <https://github.com/fuel/fuel/blob/8bdfa36e2173ed2afeb28455760cf4bfe68f96ff/CHANGELOG.md>`__

.. _release/migrate_from_chiba.2_to_d/wijmo:

Wijmo from 2013v1.4 to 2013v3.20
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Take a look of release notes of Wijmo between 2013v3.20 and 2013v1.4: http://wijmo.com/wiki/index.php/Version_Histories

.. _release/migrate_from_chiba.2_to_d/migrations.enabled_types.metadata:

End of support for config key migrations.enabled_types.metadata
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The config key ``migrations.enabled_types.metadata`` is no longer supported,
and method ``Migration::canUpdateMetadata()`` no longer exist.
During migration, all files in :file:`local/metadata` are supposed be writable.

A new event :ref:`migrate.exception <api:php/events/migrate.exception>` is triggered if a migration throws an exception.
This event can stop exception propagation.

Deprecated
----------

Those updates are not mandatory but desirable to be able to migrate without trouble when next version is released.

.. _release/migrate_from_chiba.2_to_d/i18n_crud_config:

Some i18n keys of CRUD config for plural forms
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The translation system of Novius OS now respect plural forms of languages. Some i18n keys of CRUD config are affected.

These keys now must contain an array of different plurals of translation, and not the translated text:

* ``deleting with N contexts``
* ``deleting with N languages``
* ``deleting with N children``
* ``deleting button N items``
* ``N items``
* ``showNbItems``

These keys are unnecessary:

* ``1 child``
* ``deleting button 1 item``
* ``deleting button 0 items``
* ``1 item``


.. _release/migrate_from_chiba.2_to_d/hmvc:

Nos::hmvc() API is simplified
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Second argument can be just an array, not an array with an ``args`` key containing an array.

Deprecated code:

.. code-block:: php

    <?php

    \Nos::hmvc('request/url/', array('args' => array($first_parameter, $second_parameter)));

Replace with:

.. code-block:: php

    <?php

    \Nos::hmvc('request/url/', array($first_parameter, $second_parameter));

.. _release/migrate_from_chiba.2_to_d/loadConfiguration:

Method \Config::loadConfiguration()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The method ``\Config::loadConfiguration()``. Use ``\Config::load()``.

Deprecated code:

.. code-block:: php

    <?php

    $config = \Config::loadConfiguration('application_name', 'file_name');
    //or
    $config = \Config::loadConfiguration('application_name::file_name');

Replace with:

.. code-block:: php

    <?php

    $config = \Config::load('application_name::file_name', true);

.. _release/migrate_from_chiba.2_to_d/applicationRequiredFromMetadata:

\Nos\Application::applicationRequiredFromMetadata() scope public
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The method ``\Nos\Application::applicationRequiredFromMetadata()`` is not intended to be called outside the ``\Nos\Application`` class.
It will become protected in future.

You can get all applications dependencies by loading the :file:`app_dependencies` metadata file.

.. code-block:: php

    <?php

    $dependencies = \Nos\Config_Data::get('app_dependencies', array());

.. _release/migrate_from_chiba.2_to_d/extends.application:

In metadata files, ``extends.application`` key
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In metadata files, the ``extends`` key containing an array with an ``application`` key is deprecated.

The ``extends`` key must contain only an array with applications names in values.

Deprecated code:

.. code-block:: php

    <?php

    return array(
        'name'    => 'Application name',
        //...
        'extends' => array(
            'application' => 'application_name',
            'extend_configuration' => false,
        ),
    );

Replace with:

.. code-block:: php

    <?php

    return array(
        'name'    => 'Application name',
        //...
        'extends' => array(
            'application_name',
        ),
    );

.. _release/migrate_from_chiba.2_to_d/extends.apps:

Config files extended by application extending mechanism
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Config files extended by application extending mechanism must be defined in a subdirectory :file:`apps/application_name/`

For sample, if your application A extends the sample.config.php file of the application B.

Deprecated location: :file:`local/applications/application_a/config/sample.config.php`

Move to: :file:`local/applications/application_a/config/apps/application_b/sample.config.php`

