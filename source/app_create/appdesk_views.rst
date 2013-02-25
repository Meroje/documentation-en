Add thumbnails view in App Desk
###############################

It is actually quite simple. You need to define two special keys in ``data_mapping``:

- ``thumbnail``: thumbnail item path ;
- ``thumbnailAlternate`` : default path when no item thumbnail path is defined.

In the file :file:`config/common/item.config.php`:

.. code-block:: php

    <?php

    return array(
        'data_mapping' => array(
            'thumbnail' => array(
                'value' => function ($item) {
                    foreach ($item->medias as $media) {
                        return $media->get_public_path_resized(64, 64);
                    }
                    return false;
                },
            ),
            'thumbnailAlternate' => array(
                'value' => function ($item) {
                    return 'static/apps/mon_appli/icons/64.png';
                }
            ),
        ),
    );


You need then to enable the thumbnails view in the App Desk configuration
:file:`my_app::config/controller/admin/appdesk.config.php` :


.. code-block:: php
   :emphasize-lines: 8

    <?php

    return array(
        'model' => '',
        'query' => array(),
        'inspectors' => array(),
        'i18n' => array(),
        'thumbnails' => true,
    );


If you want to show the thumbnails view by default:

.. code-block:: php
   :emphasize-lines: 9-13

    <?php

    return array(
        'model' => '',
        'query' => array(),
        'inspectors' => array(),
        'i18n' => array(),
        'thumbnails' => true,
        'appdesk' => array(
            'appdesk' => array(
                'defaultView' => 'thumbnails',
            ),
        ),
    );
