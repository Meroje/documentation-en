Create a template
#################

1. :file:`metadata` configuration
=================================

Template metadata are described in :ref:`the API documentation <api:metadata/templates>`.


2. View file creation
=====================

File location depends on the ``file`` key configured in the :file:`metadata.config.php` file.

Inside the template, some variable can be accessed:

:$wysiwyg: A hash which keys are the WYSIWYG name configured in the :file:`metadata.config.php` file and values are
  		   content the user entered.
:$page: ``Nos\model_Page`` instance.
:$main_controller: :ref:`Front controller instance <api:php/classes/controller_front>`.

.. code-block:: html

    <!DOCTYPE html>
    <html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <link rel="shortcut icon" href="static/favicon.ico" />
        <link rel="stylesheet" type="text/css" href="static/apps/noviusos_templates_basic/css/base.css" media="all">
    </head>

    <body>

        <header>This header will be displayed on all pages configured to use this template.</header>

        <div id="menu">
        </div>

        <div id="content">
            <?= $wysiwyg['content']; ?>
        </div>

    </body>
    </html>
