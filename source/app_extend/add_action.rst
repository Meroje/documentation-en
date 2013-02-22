Add an action in the admin
##########################


Actions are defined in the :file:`config/common/{{model}}.config.php` file.

The best way to proceed is to be inspired by the default existing actions of Novius OS.


Placeholders
************

Action's configuration contains ``{{placeholders}}``.

- Replaced with **PHP** :

    - ``{{model_label}}``: the model name
    - ``{{controller_base_url}}``: URL of the model's controller

- Replaced by the **App Desk** (JavaScript) :
    - ``{{context}}``: current context (or first one when several are shown)

Every other placeholders are replaced according to the data of the **item**: ``{{_id}}`` and ``{{_title}}`` in this
case, but also any field defined in the ``data_mapping``.


Action's target
***************

There are 3 possibles targets for the actions:

- **toolbar-grid**: App Desk's toolbar
- **grid**: item line in the main grid of the App Desk
- **toolbar-edit**: toolbar on the editing / addition form


.. image:: images/targets_grid.png
	:alt: 'grid' target of actions
	:align: center

.. image:: images/targets_edit.png
	:alt: 'edit' target of actions
	:align: center



Add / Edit / Delete
*******************

**add** action:

- opens a new tab ;
- calls the ``action_insert_update()`` method on the ``Nos\Media\Controller_Admin_Media`` controller ;
- with the ``$_GET['context']`` parameter, allowing to pre-select the active context ;
- is only shown in the App Desk's toolbar.

**edit** action:

- opens the edition form (the method is not specified for ``nosTabs``, so the default ``open`` value will be used: it
  will focus the existing tab if it's already opened, or will create a new one otherwise ;
- calls the ``action_insert_update( $id )`` method on the ``Nos\Media\Controller_Admin_Media`` controller ;
- with an ``id`` parameter ;
- is only shown in the main grid.

**delete** action:

- calls the ``action_delete( $id )`` method on the ``Nos\Media\Controller_Admin_Media`` controller ;
- with an ``id`` parameter ;
- is shown both in the main grid and the edition form, but only for existing items (not for adding new items).

.. code-block:: php
   :emphasize-lines: 5,23,42

    <?php

    return array(

        // Default ADD action
        'add' => array(
            'label' => __('Add {{model_label}}'),
            'primary' => true,
            // Opens a new tab on click
            'action' => array(
                'action' => 'nosTabs',
                'method' => 'add',
                'tab' => array(
                    'url' => '{{controller_base_url}}insert_update?context={{context}}',
                ),
            ),
            // The ation is only be shown in the App Desk's toobar
            'targets' => array(
                'toolbar-grid' => true,
            ),
        ),

        // default EDIT action
        'edit' => array(
            'label' => __('Edit'),
            'primary' => true,
            'icon' => 'pencil',
            // Opens the item on click (will refocus the tab when existing)
            'action' => array(
                'action' => 'nosTabs',
                'tab' => array(
                    'url' => "{{controller_base_url}}insert_update/{{_id}}",
                    'label' => '{{_title}}',
                ),
            ),
            // The action is only be shown in the main grid
            'targets' => array(
                'grid' => true,
            ),
        ),

        // Default DELETE action
        'delete' => array(
            'label' => __('Delete'),
            'primary' => true,
            'icon' => 'trash',
            'red' => true,
            // Opens a confirmation popup on click
            'action' => array(
                'action' => 'confirmationDialog',
                'dialog' => array(
                    'contentUrl' => '{{controller_base_url}}delete/{{_id}}',
                    'title' => strtr($config['i18n']['deleting item title'], array(
                        '{{title}}' => '{{_title}}',
                    )),
                ),
            ),
            // The action is shown both in the main grid and the edition form...
            'targets' => array(
                'grid' => true,
                'toolbar-edit' => true,
            ),
            // ...but not for new items!
            'visible' => function($params) {
                return !isset($params['item']) || !$params['item']->is_new();
            },
        ),
    );
