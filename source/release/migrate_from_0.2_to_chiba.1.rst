Migration guide from the 0.2 version to the Chiba 1 version
###########################################################

Few changes are needed to migrate to the next version. The new API is compatible with the old one. From this version,
Novius OS handles depreciations. Here a deprecated items from Chiba 1:

* ``Nos\Renderer_Media`` should be renamed to ``Nos\Media\Renderer_Media``.
* Launchers configuration: the ``url`` key is deprecated. Use 'action' instead.
* The ``widget`` key is deprecated in renderer configuration. Use the ``renderer`` key and update the class name.
* The ``widget_options`` key is deprecated in renderer configuration. Use the ``renderer_options`` key.
* ``\Config::extendable_load()`` is deprecated. Renamed to ``\Config::loadConfiguration()``.
* ``Orm_Behaviour_Publishable`` configuration: the ``publication_bool_property`` key is deprecated. Use
  ``publication_state_property`` instead.

Pages are now cached by default (for 10 minutes) ; it could result to unexpected behaviour for custom applications using
enhancer and / or urlenhancer.

Some migration files need to be executed. If you use ``oil``, the new command is ``php oil refine migrate -m`` as
migration files are now organised differently.
