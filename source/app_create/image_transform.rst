Display thumbnails in differents formats
########################################

You have added thumbnails to your model. Now you want to displaying them in front-office.

In list mode, you want to displaying them cropped to 150x150 pixels and in grayscale.

In your list view:

.. code-block:: php
   :emphasize-lines: 4-6

    <?php

    foreach ($items as $item) {
        echo $item->thumbnail->getToolkitImage()->crop_resize(150, 150)->grayscale()->html(array(
            'style' => 'float:right;'
        ));
        echo '<h2><a href="', $item->url(), '">', e($item->title), '</a></h2>';
    }

In item page, you want to display the thumbnail with a max width of 300 pixels and a max height of 200 pixels,
and a rotation of 15 degrees.

In your item's view:

.. code-block:: php
   :emphasize-lines: 3

    <?php

    echo $item->thumbnail->getToolkitImage()->shrink(300, 200)->rotate(15)->html();
    echo '<h1>', e($item->title), '</h1>';
    echo '<p>', e($item->description), '</p>';

.. seealso::    :ref:`Toolkit_Image class for more possibilities <api:php/classes/toolkit_image>`.