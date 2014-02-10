Updates
#######

Updating the files
******************


Git
====

If you installed Novius OS with :program:`Git`, run these commands from Novius OS directory:

.. code-block:: bash

	git fetch origin
	git checkout master/dubrovka
	git submodule update --recursive --init

.. note::

    The ``submodule update`` can display a line

    .. code-block:: bash

        warning: unable to rmdir packages/log: directory not empty

    in this case, execute this command

    .. code-block:: bash

        rm -rf novius-os/packages/log/

Zip
====

If you downloaded the Zip file, the procedure is more complex.


* Let's say you installed novius OS in :file:`my_site/`.
* First, backup your old directory (copy it and make a .zip file with it).
* Download the `new Zip of Novius OS <http://www.novius-os.org/download-novius-os-zip.html>`__ and extract it. You now
  have a :file:`novius-os/` directory.
* In :file:`my_site/`, delete the following directories:
	* :file:`my_site/novius-os/`
	* Every :file:`my_site/local/applications/noviusos_*` directories

* Copy the following directories and files from :file:`novius-os/` to :file:`my_site/` :
	* :file:`novius-os/`
	* Tous les répertoires :file:`local/applications/noviusos_*`

	* :file:`novius-os/`
	* Every :file:`local/applications/noviusos_*` directories
	* Every :file:`local/config/*.sample` files
	* :file:`public/htdocs/install/`, :file:`public/htdocs/install.php` and :file:`public/htdocs/migrate.php.sample`
	* Every files in root directory

Now you can continue the update.

.. _install/upgrade/migration:

Run the migration
*****************

Before running the automated migration tools, please backup your database.

Via SSH
=======

If you're allowed to acces :program:`SSH` on the server, run this command from your Novius OS directory:

.. code-block:: bash

	sudo php oil refine migrate
	sudo php oil refine migrate -m

Via Browser
===========

If you can't access :program:`SSH`, you can run the migration from your browser:

* First, you need to rename the :file:`public/migrate.php.sample` file to :file:`public/migrate.php`.
* Open the file in your browser, such as :file:`http://www.my_site.com/migrate.php`.

Via back-office
===============

If you can't access :program:`SSH`, you can run the migration from back-office of your Novius OS:

* Connect to your back-office
* Open the "Applications manager" application
* Click on "Apply changes" for each applications, or on "Refresh all metadata" in toolbar if your in expert mode.

.. warning::

    When you access to your back-office without migrated, your software is in a instable state.
    Sources not matches with the DB state. You will probably see error messages.
    You can ignore them.

Migrate your developments
*************************

If you have personnal developments, you need to follow the :doc:`/release/migrate_from_chiba.2_to_dubrovka`.


