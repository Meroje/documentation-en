Updates
#######

Updating the files
******************


Git
====

If you installed Novius OS with :program:`Git`, run these commands from Novius OS directory:

.. code-block:: bash

	git fetch origin
	git checkout master/0.2
	git merge origin/master/0.2
	git submodule update --recursive --init

Zip
====

If you downloaded the Zip file, the procedure is more complex.


* Let's say you installed novius OS in :file:`my_site/`.
* First, backup your old directory (copy it and make a .zip file with it).
* Download the `new Zip of Novius OS <http://www.novius-os.org/download-novius-os-zip.html>`__ and extract it. You now
  have a :file:`novius-os/` directory.
* In :file:`my_site/`, delete the following directories:
	* :file:`my_site/novius-os/`
	* :file:`my_site/local/migrations/`
	* Every :file:`my_site/local/applications/noviusos_*` directories

* Copy the following directories and files from :file:`novius-os/` to :file:`my_site/` :
	* :file:`novius-os/`
	* :file:`local/migrations/`
	* Every :file:`local/applications/noviusos_*` directories
	* :file:`public/install.php`
	* :file:`public/.htaccess`

You may need :file:`.sample` files later in the procedure. If so, copy them from the :file:`novius-os/` to
:file:`my_site/` when asked.

Now you can continue the update.

Run the migration
*****************

Before running the automated migration tools, please backup your database.

If you're allowed to acces :program:`SSH` on the server, run this command from your Novius OS directory:

.. code-block:: bash

	sudo php oil refine migrate

| If you can't access :program:`SSH`, you can run the migration from your browser:

* First, you need to rename the :file:`public/migrate.php.sample` file to :file:`public/migrate.php`.
* Open the file in your browser, such as :file:`http://www.my_site.com/migrate.php`.


Applications Manager
********************

From the back-office of Novius OS, open the ``Applications manager`` and update the applications which need it.


Migrate your developments
*************************

If you have personnal developments, you need to follow the :doc:`/release/migrate_from_0.1_to_0.2`.


