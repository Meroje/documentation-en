Post-install optimisations
==========================


Configure and enable XSendFile
------------------------------

To undestand what XSendFile is and its usefulness, go here : :doc:`/understand/media_centre`.


Apache
~~~~~~

**Apache** provides the feature through the **mod_xsendfile** module. Then, the .htaccess must be updated to
enable it:

.. code-block:: apache

    # Post-installation optimisation
    <IfModule xsendfile_module>
        XSendFile On

        # Replace "novius-os-install-dir" by the real Novius OS installed directory
        XSendFilePath /novius-os-install-dir/local/data

    </IfModule>

Novius OS know when the module is enabled automatically and enable the XSendFile feature accordingly.


nginx
~~~~~

**nginx** provides the feature natively, but the header used is ``X-Accel-Redirect``. In that case, you need to edit
the :file:`config.php` file to tell Novius OS the appropriate header :

.. code-block:: php
    :emphasize-lines: 6

    <?php
    return array(
        // ...
        'novius-os' => array(
            // ...
            'use_xsendfile' => 'X-Accel-Redirect',
        ),
    );


