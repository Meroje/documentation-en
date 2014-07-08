Install a Novius OS application
###############################

Where to find apps
******************

`Novius OS’ GitHub account <http://github.com/novius-os>`__ is a good place to start.

Check out the `contributors’ page <http://community.novius-os.org/Get-involved/our-awesome-contributors.html>`__ on Novius OS website for applications from the community.

You could also go straight to `Fumito Mizuno <http://github.com/ounziw>`__ and `Novius Agency <http://github.com/novius>`__’s GitHub accounts which feature many apps.


Install a new application
*************************

2 solutions are available: using **Git** or a **.zip file**.


1st method : using Git
======================

On GitHub, copy the repository URL:

.. image:: images/download_git.png

Then, clone the repository in the :file:`/local/applications/` directory.

.. code-block:: bash

    cd local/applications
    git clone REPOSITORY_URL

Lastly, don't forget to :ref:`activate the application <manage/install_app/activate>` in the applications manager.


2nd method: using a .zip file
=============================

On GitHub, download the application as a **.zip** file:

.. image:: images/download_zip.png

Unzip the downloaded archive in the :file:`/local/applications/` directory.

Rename the created directory in order to delete the branch name (which is automatically added by GitHub).
For example, you will rename :file:`novius_ftplite-master-elche` into :file:`novius_ftplite` only.


Lastly, don't forget to :ref:`activate the application <manage/install_app/activate>` in the applications manager.

.. warning::

    Don't modify the actual files inside the application you just downloaded, or you won't be able to update it later!
    Please use the :doc:`extensions mechanisms </app_extend/index>` in order to change how it behaves.


.. _manage/install_app/activate:

Activate an application
***********************

Open the applications manager (from the desktop) :

.. image:: images/app_manager_launcher.png

Clic on :guilabel:`« Install »` next to the name of your application.

.. image:: images/activate_app.png


Update an application
*********************


1st method: using Git
=====================

Go into the directory containing your application, and update the repository in the desired version:

.. code-block:: bash

    cd local/applications/novius_ftplite
    git fetch
    git checkout master/elche

Then, go in the applications manager to :guilabel:`« Apply changes »`.


2ns method: from a .zip file
============================

.. note::

    Before updating an application, check that your (potential) specific developmens are compatibles.

On GitHub, download the new version of the application as a **.zip** file.

Then, replace the corresponding directory in :file:`local/applications` (you can delete the old one to put the new one).

As when installing, don't forget to rename the directory in order to delete the branch name (which is automatically added by GitHub).

Then, go in the applications manager to :guilabel:`« Apply changes »`.
