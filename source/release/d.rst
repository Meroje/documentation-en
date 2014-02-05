Release notes D
#####################

New features
============

* Applications can extend multiple applications
* The extend application mechanism works also for views and lang files, not only for config files
* Russian translations
* Spanish translations
* Interlingue (Occidental) translations
* Japanese translations updated

Developer
=========

Breaking changes
----------------

* :ref:`release/migrate_from_chiba.2_to_d/fuelphp`
* :ref:`release/migrate_from_chiba.2_to_d/wijmo`
* :ref:`release/migrate_from_chiba.2_to_d/migrations.enabled_types.metadata`

Vendors update
--------------

* FuelPHP 1.7.1
* Wijmo 2013v3.20
* jQuery 1.10.2
* require.js 2.1.9
* TinyMCE 3.5.10
* JQuery.cookie 1.4
* JQuery.form 3.46.0-2013.11.21
* JQuery.validation 1.11.1
* JQuery.mousewheel 3.1.6
* JQuery.datetimepicker 1.4.3

Improvements
------------

* **UI** Integration of the Novius OS new logo
* **UI**: The rendering is improved when switching tab before end of loading
* **Install**: Improving messages in the step one of the install wizard for facilitate understanding of the potential problems
* **i18n**: Add plural mechanism and implement plural translation
* **Renderer**: Selector for pages can now send multiple pages using checkboxes
* **Renderer**: Add a generic Renderer::renderer() method for all renderers that extended Renderer
* **WYSIWYG**: Refactoring TinyMCE Novius OS specific features. Explode all features in plugins, much more modular.
* **404**: allow to use novius_ftplite app to add custom robots.txt (favicon or humans.txt)
* | **Behaviour**: All aliases in ``where`` and ``order_by`` options of ``find()`` work whatever the level where the alias is used and even in chaining methods.
  | Concern: ``context`` in ``Contextable``, ``published`` in ``Publishable``, ``default_sort`` in ``Sortable``, ``parent`` in ``Tree`` and ``context_main`` in ``Twinnable``.
* **Migration**: Add an incremental ID and an execution date in migration table
* **App manager**: Buttons are disabled after the click to prevent to recall the same action two times
* **Blog/News**: Add a specific title for an author's posts list
* **Blog/News**: Changing page_title and meta title for posts list of category, tag and author
* **Comments**: The comment context can be passed by parameters in API

Deprecated
----------

* :ref:`release/migrate_from_chiba.2_to_d/i18n_crud_config`
* :ref:`release/migrate_from_chiba.2_to_d/hmvc`
* :ref:`release/migrate_from_chiba.2_to_d/loadConfiguration`
* :ref:`release/migrate_from_chiba.2_to_d/applicationRequiredFromMetadata`
* :ref:`release/migrate_from_chiba.2_to_d/extends.application`
* :ref:`release/migrate_from_chiba.2_to_d/extends.apps`
