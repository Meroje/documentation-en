Release notes Elche
###################

New features
============

* New **Menu application** to manage website menu.
* New **Template variation application** to manage template variations by context.
* New **Bootstrap customisable template application** which use power of template variation. You can customize elements, layout, skin, menus.
* **UI**: Background image on body and sides always visible
* **UI**: New tray bar with new fullscreen feature
* **UI**: Appdesk and CRUD Toolbar at bottom
* **UI**: On Appstab, launchers now draggable on a grid, not only sortable
* **Renderer**: New renderer ``Renderer_Item_Picker`` based on native appdesk of model
* **Renderer**: New renderer ``Renderer_Select_Model`` to select item of a model. Manage contexable and twinnable behaviour.

Developer
=========

* **ORM**: Add Providers feature. Possibility to add providers on model. Providers are accessors to relation's items by a key.
* **ORM**: Wysiwygs and Medias becomes native providers.
* **ORM**: New ``shared_wysiwygs_context`` and ``shared_medias_context`` providers.
* **Enhancer**: Config popup now run form validation if some fields have contraints.
* **Grid**: ``cellFormatters`` on columns work on all ``noslistgrid`` widget, not only on principal grid of appdesk

Breaking changes
----------------

* :ref:`release/migrate_from_dubrovka_to_elche/shared_wysiwygs_medias`

Vendors update
--------------

* jQuery 1.11.1
* jQuery UI 1.10.4
* Wijmo 2014v1.34
* require.js 2.1.11
* pNotify 2.0
* JQuery.cookie 1.4.1
* JQuery.form 3.50.0-2014.0.2.05
* JQuery.mousewheel 3.1.11
* JQuery.datetimepicker 1.4.4
* Webshims 1.13, in Form application,

Deprecated
----------

* :ref:`release/migrate_from_dubrovka_to_elche/nos_methods`
