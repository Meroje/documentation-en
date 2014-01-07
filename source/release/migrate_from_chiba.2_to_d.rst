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

A new event ``migrate.exception`` is triggered if a migration throws an exception.
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
