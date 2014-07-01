Migration guide from the Dubrovka version to the Elche version
###############################################################

Upgrade your Novius OS and its applications
*******************************************

See :doc:`/install/upgrade` page if you didn't already do it.

Migrate your developments
**************************

Breaking changes
----------------

.. _release/migrate_from_dubrovka_to_elche/shared_wysiwygs_medias:

Shared Wysiwygs and Medias context
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* All shared Wysiwygs and Medias context must be access with providers ``shared_wysiwygs_context`` and ``shared_medias_context``
* Remove declaration of variables ``shared_wysiwygs_context`` and ``shared_medias_context`` from model

Deprecated
----------

Those updates are not mandatory but desirable to be able to migrate without trouble when next version is released.

.. _release/migrate_from_dubrovka_to_elche/nos_methods:

Nos:parse_wysiwyg(), Nos:parse_enhancers() and Nos:get_enhancer_content()
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

These methods are deprecated, use:

* ``Tools_Wysiwyg::parse()`` instead of ``Nos:parse_wysiwyg()``
* ``Tools_Wysiwyg::parseEnhancers()`` instead of ``Nos:parse_enhancers()``
* ``Tools_Enhancer::content()`` instead of ``Nos:get_enhancer_content()``
