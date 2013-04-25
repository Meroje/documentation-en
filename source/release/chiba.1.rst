Release notes Chiba 1
#####################

News features
=============

* The behaviour publishable (``Behaviour_Publishable``) now allows to choose publication start and end dates.

Form
----

* Adding a progress bar on multi-page form.

Blog / News
-----------

* Display related posts of authors.

Developer
=========

* Front improvements

    * ``Profiling`` is activated by default on front in the ``DEVELOPMENT`` environment.
    * Setting configuration ``novius-os.cache`` by default always at true.
    * Setting configurations ``novius-os.cache_duration_page`` and ``novius-os.cache_duration_function`` at 600 by default, except in ``PRODUCTION`` at 3600.
    * New events: ``front.pageFound`` and ``front.response``.
    * New methods in ``Controller_Front``: getContext, disableCaching, setCacheDuration, setStatus, setHeader, getCustomData, setCustomData, sendContent, addCacheSuffixHandler.
    * Status and headers are now save in cache.
    * Mechanism to adapt the cache path with suffixes, depending GET parameters or what you want (with ``callables``).
    * Mechanism to execute code when using the cache.

* Models properties

    * All models (core and native apps) now defines the $_properties
    * Implement a cache mechanism on models properties, using FuelPHP cache. Attempt to auto-refresh when an unknown properties is called.

* Migrations are now dispatched per application.
* New ``metadata`` key ``requires`` which allows to define that an application requires another one.
* It is now possible to use ``href="##..."`` in enhancers or templates; occurences will be replaced by ``href="#..."`` without prepending the ``base_url``.
* CRUD: When returning a string in the ``disabled`` key, it is displayed as a title. ``disabled`` and ``visible`` keys can now be simple values, callbacks or array of callbacks.
* Resized images are now secured: you can't generate a lot of thumbnails and flood the server anymore.


* Permissions

    * Ability to define per-application permissions with a configuration file
    * New API to check permissions for a user, or a specific role
    * Ability to enable multi-roles on the users with the ``novius-os.users.enable_roles`` configuration.


Deprecated
----------

* Moved ``Nos\Renderer_Media`` to ``Nos\Media\Renderer_Media``.
* Launchers configuration: the ``url`` key is deprecated. Use ``action`` instead.
* The ``widget`` key is deprecated in renderer configuration. Use the ``renderer`` key and update the class name.
* The ``widget_options`` key is deprecated in renderer configuration. Use the ``renderer_options`` key.
* ``\Config::extendable_load()`` is deprecated. Renamed to ``\Config::loadConfiguration()``.
* ``Orm_Behaviour_Publishable`` configuration: the ``publication_bool_property`` key is deprecated. Use ``publication_state_property`` instead.
