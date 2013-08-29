Migration guide from the Chiba 1 version to the Chiba 2 version
###############################################################

Upgrade your Novius OS and its applications
*******************************************

See :doc:`/install/upgrade` page if you d'ont already do it.

Migrate your developments
**************************

Breaking changes
----------------

.. _release/migrate_from_chiba.1_to_chiba.2/model_dataset:

Model: columns of ``dataset`` now are encoded
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If a column of ``dataset`` contains HTML, you must add a key ``isSafeHtml`` for not encode it.

.. code-block:: php

    <?php
    return array(
        'data_mapping' => array(
            // ...
            'column_with_html' => array(
                'title' => 'Column with HTML',
                'column' => 'col_html',
                'isSafeHtml' => true,
            ),
            // ...
        ),
    );

.. seealso:: :ref:`Common configuration for Model <api:php/configuration/application/common>`

.. _release/migrate_from_chiba.1_to_chiba.2/crud_success:

CRUD: ``success`` callback is called after ``save``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In CRUD, when updating an item, ``success`` callback function is called after ``save`` (not before), like in inserting.
If you use ``success`` in your developments, check that your code is compatible with a call after ``save``.

.. _release/migrate_from_chiba.1_to_chiba.2/attachment:

Attachment: ``->url()`` and ``->urlResized()`` return absolute URLs
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Now, ``->url()`` and ``->urlResized()`` methods return absolutes URLs. You have two choices for update your developments:

* check that you're don't concatenate ``base_url`` before calls of those methods.
* Add a parameter equal to ``false`` in call of method.

    .. code-block:: php

        <?php

        $attachement->url(false);

.. seealso:: :ref:`api:php/classes/attachment`

.. _release/migrate_from_chiba.1_to_chiba.2/comments:

Comments: now comments are ``contextable``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Migration tries to guess context of existing comments, but if you've implement comments on a not contextable model,
migration can't do nothing. In this case, set yourself context (``comm_context`` column of ``nos_comment`` table)
if you want to see those comments in new administration interface.

.. _release/migrate_from_chiba.1_to_chiba.2/blognews:

Blog/News: the default size of thumbnails have change and they are clickable
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* The default size of thumbnails have change from 200 to 120 pixels on list, always 200 on item.
* Thumbnails are clickable.

If you want to revert to the previous configuration:

* Extend the related configuration file :file:`noviusos_blog::config` or :file:`noviusos_news::config`
* Edit configuration like that:

    .. code-block:: php

        <?php

            return array(
                'thumbnail' => array(
                    'front' => array(
                        'list' => array(
                            'link_to_item' => false,
                            'max_width' => 200.
                        ),
                        'item' => array(
                            'link_to_fullsize' => false,
                        ),
                    ),
                ),
            );

Deprecated
----------

Those updates are not mandatory but desirable to be able to migrate without trouble when next version.

.. _release/migrate_from_chiba.1_to_chiba.2/enhancer:

Enhancer: ``get_url_model($item, $params)`` becomes ``getURLEnhanced($params)``
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Deprecated code:

.. code-block:: php

    <?php

    public static function get_url_model($item, $params = array())
    {
        $model = get_class($item);

        switch ($model) {
            case 'A\Class':
                return $item->virtual_name).'.html';
                break;
        }

        return false;
    }

Replace with:

.. code-block:: php

    <?php

    public static function getURLEnhanced($params = array())
    {
        $item = \Arr::get($params, 'item', false);
        if ($item) {
            $model = get_class($item);

            switch ($model) {
                case 'A\Class':
                    return $item->virtual_name).'.html';
                    break;
            }
        }

        return false;
    }

.. _release/migrate_from_chiba.1_to_chiba.2/media:

Media: Changes in Model_Media API
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

All ``snake_case`` methods are deprecated:

* ``delete_from_disk`` becomes ``deleteFromDisk``
* ``delete_public_cache`` becomes ``deleteCache``
* ``get_path`` becomes ``_getVirtualPath``
* ``get_private_path`` becomes ``path``
* ``get_img_tag`` becomes ``htmlImg``
* ``get_img_tag_resized`` becomes ``htmlImgResized``
* ``is_image`` becomes ``isImage``
* ``get_public_path`` becomes ``url``
* ``get_public_path_resized`` becomes ``urlResized``

.. seealso:: :ref:`api:php/models/media/model_media/methods`

.. _release/migrate_from_chiba.1_to_chiba.2/media_folder:

Media: Changes in Model_Folder API
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* ``delete_from_disk`` becomes ``deleteFromDisk``
* ``delete_public_cache`` becomes ``deleteCache``

.. seealso:: :ref:`api:php/models/media/model_folder/methods`

.. _release/migrate_from_chiba.1_to_chiba.2/page_link:

Page: Model_Page->link() deprecated
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

``Model_Page->link()`` is deprecated, use ``Model_Page->htmlAnchor()`` instead.

.. warning::

    ``Model_Page->link()`` returns only ``href`` and ``target`` attributs, ``Model_Page->htmlAnchor()``
    returns the whole HTML tag ``<a>``.

.. seealso:: :ref:`api:php/models/model_page/methods`

.. _release/migrate_from_chiba.1_to_chiba.2/user_login:

Event ``user_login``
^^^^^^^^^^^^^^^^^^^^

The ``user_login`` event is deprectaed, use ``admin.loginSuccess`` instead.

.. seealso:: :ref:`api:php/events/admin.loginSuccess`

