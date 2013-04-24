Permissions
###########


Roles (or "profiles")
=====================

Permissions are always applied to a role. Then, each user is given one or many roles, from which the permissions are
determined.


One role by user
----------------

For the sake of simplicity and understanding, on a default installation:

- these roles are hidden ;
- each user gets one unique role attributed ;
- it's not possible to share a role among several users.

Hence the role notion is completely hidden and the permissions are configured on a dedicated tab of the user form :

.. image:: images/user_standalone.png
    :align: center


Several roles by user
---------------------

Meanwhile it's possible to enable multiple roles in the main configuration file :
``novius-os.users.enable_roles = true``.

Once enable, it becomes possible to share a role among several users. Permissions can then be configured more precisely:

- on the user form, the "Permissions" tab disappear in favor of a "Roles" block ;
- the AppDesk of the Users application gains:

  - a new "Roles" inspector ;
  - a new "Add a role" action.

- permissions should now be configured on the role form.


.. image:: images/user_roles.png
    :align: center



Permission structure
====================

They are two types of permission:

- simple: yes or no ;
- multiples : applied on a list of categories (values);


A **simple** permission has a meaning just by itself. For instance "Can I add a page?" or "Can I delete a locked page?".

A **multiple** permissions don't have a meaning just by themselves, and can only be be expressed depending on a category
(value). For instance with "Can I write in this folder?" we need a list of folders for the permission to have a meaning.
With "Can I access this application?" we need a list of applications.

A **simple** permission is composed of a unique ``perm_name`` column, whereas a **multiple** role (using categories) is
composed of two columns: ``perm_name`` (the same on as a simple permission) and ``perm_category_key`` (the value).


How-to use permissions in the applications
==========================================

:file:`permissions.config.php` file
-----------------------------------

By creating this file, each application can define its own list of permissions it needs.

They can be configured in the dedicated column when editing the permissions :

.. image:: images/permissions.png
    :align: center


.. seealso:: :ref:`associated API for the permission configuration file <api:php/configuration/application/permissions>`


API to check a permission
-------------------------

.. code-block:: php

    <?php
    // Simple: only 1 argument, the permission name
    Permission::check('noviusos_app::delete_locked');

    // Multiple (uses categories) : 2 arguments, the permission name + the category key
    Permission::check('noviusos_app::create_in_folder', $folder_id);

.. seealso:: API documentation of the :ref:`Permission <api:php/classes/permission>` class.

.. warning::

    The permission name is an important thing. The part preceding ``::`` must refer to an valid application. For the
    permission to be granted, the user also needs to have access to this application.

CRUD
----

It's possible to hide some fields based on the permissions. To do so, the :ref:`show_when <api:php/configuration/application/crud/fields>`
key defines a callback function returning whether the field should be visible or not.

.. code-block:: php
   :emphasize-lines: 9-12

    <?php
    return array(
        'fields' => array(
            'my_field' => array(
                'label' => 'My field',
                'form' => array(
                    'type' => 'text',
                ),
                'show_when' => function() {
                    // The field will only be visible when the user has the requested permission
                    return Permission::check('my_app::my_permission');
                },
            ),
        ),
    );

Actions
-------

It's possible to disabled actions based on the permissions using the :ref:`disabled <api:php/configuration/application/common/actions>` key.

.. code-block:: php
   :emphasize-lines: 13-17

    <?php
    return array(
        'data_mapping' => array(/*...*/),
        'actions' => array(
            'delete' => array(
                'label' => __('Delete'),
                'primary' => false,
                'icon' => 'home',
                'action' => array(/*...*/),
                'targets' => array(
                    'grid' => true,
                ),
                'disabled' => array(
                    function($item) {
                        return !Permission::check('my_app::can_delete_item') ? __('You don\'t have the permission to delete items.') : false;
                    }
                ),
            ),
        ),
    );

