Release notes D
#####################

New features
============

* Russian translations
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

* jQuery 1.10.2
* JQuery.cookie 1.4
* JQuery.datetimepicker 1.4.3

Improvements
------------

* **i18n**: Add plural mechanism and implement plural translation
* **Renderer**: Selector for pages can now send multiple pages using checkboxes
* **WYSIWYG**: Refactoring TinyMCE Novius OS specific features. Explode all features in plugins, much more modular.
* **404**: allow to use novius_ftplite app to add custom robots.txt (favicon or humans.txt)
* | **Behaviour**: All aliases in ``where`` and ``order_by`` options of ``find()`` work whatever the level where the alias is used and even in chaining methods.
  | Concern: ``context`` in ``Contextable``, ``published`` in ``Publishable``, ``default_sort`` in ``Sortable``, ``parent`` in ``Tree`` and ``context_main`` in ``Twinnable``.
* **Migration**: Add an incremental ID and an execution date in migration table

Deprecated
----------

* :ref:`release/migrate_from_chiba.2_to_d/i18n_crud_config`
