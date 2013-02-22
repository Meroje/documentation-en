Extensions mechanisms
#####################


Create a file in :file:`local`
******************************

Any view or configuration file can be changed via the :file:`local` folder.

This is possible thanks to the cascading file system existing in FuelPHP and adapted for Novius OS. It's very easy to
do, because you only need to copy an existing file and change it how you like!

For an application, the file should be copied into :file:`local/config/apps/{{application}}/` or
:file:`local/views/apps/{{application}}/`.

To extend a file from the core of Novius OS, we'll use :file:`local/config/apps/novius-os/` and
:file:`local/views/apps/novius-os/`.

The 'generic' pattern is :file:`local/{{section}}/{{application}}/` with:

* :file:`{{section}}`` equals to :file:`config` or :file:`views` ;
* :file:`{{application}}`` matching :file:`apps` + an application name or :file:`novius-os` for the core.

Configuration
=============

The ``noviusos_page`` application has a :file:`controller/admin/appdesk.config.php` configuration file (so it's located
at :file:`noviusos_page::config/controller/admin/appdesk.config.php`).

If we copy it into :file:`local/{config/apps}/noviusos_page/controller/admin/appdesk.config.php`, then it will be
merged automatically with the one asked by the application.

Views
=====

When we create a :file:`local/views/{apps/noviusos_help}/admin/help.view.php` file, it will be used as a
**replacement** of :file:`noviusos_help::admin/help.view.php`!

To extend a file from the core, we'll use ``novius-os`` as application name. For example,
:file:`local/views/novius-os/admin/login.view.php`.


Use events to alter a configuration
***********************************

Any configuration file can be altered thanks to the :ref:`events_configuration`. event.


Replace a view with another one
*******************************

It's possible to call the ``View::redirect()`` method to replace any view file by another one.


.. code-block:: php

    <?php

    // Replace the 'admin/help' view of the 'noviusos_help' application by the 'help' view of the 'local' directory
    View::redirect('noviusos_help::admin/help', 'local::help');


Create a dedicated extension application
****************************************

To extend an application, a dedicated application can be created, which will alter how the first one works.

The second application defines its extending ``my_application`` through its :file:`metadata.config.php` file:

.. code-block:: php
   :emphasize-lines: 5-6

    <?php

    return array(
        'name' => 'Application 2',
        // It's an extension application
        'extends' => 'my_application',
    );


Once ``application_2`` is installed, it will be loaded at the same time than ``my_application`` is.

When an application extends another one, some automatic behaviours falls into place.


**Example:**

``application_2`` extends ``my_application``.

Configuration files of ``Controller`` and ``Model`` inside ``my_application`` can automatically be extended by ``application_2`` just by creating them at the same location.

For instance, ``my_application`` has the following configuration file for ``Controller_Test``: :file:`applications/mon_application/config/controller/test.config.php`.

In ``application_2``, if the matching file :file:`applications/application_2/config/controller/test.config.php` exists, then it will be merged.

i.e. in ``My\Application\Controller_Test``, the ``$config`` variable will contain the merge of the 2 files (the one of
the extended ``my_application`` application, and also the one from ``application_2`` which extends the first one).


