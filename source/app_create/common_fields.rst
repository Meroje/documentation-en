Add commun fields to all contexts
#################################

If your application is ``Twinnable``, you may want to have some fields of a model common to all contexts.
For example, in ``Monkey`` application, the birth year, species and photo of a monkey are not context dependants.
A monkey in french version will have the same birth year, the same species and the same photo that in english version.

.. seealso:: :doc:`/understand/multi_context/index`

Common field
************

For define a common field, just adds it to ``common_fields`` key of ``Twinnable`` behaviour.

Example
*******

.. code-block:: php

    <?php
    class Model_Page extends \Nos\Orm\Model
    {
        protected static $_behaviours = array(
            'Nos\Orm_Behaviour_Twinnable' => array(
                'context_property'      => 'monk_context',
                'common_id_property' => 'monk_context_common_id',
                'is_main_property' => 'monk_context_is_main',
                'common_fields'   => array('monk_species_common_id', 'monk_birth_year'),
            ),
        );
    }

Common media and WYSIWYG
************************

For define a common media or WYSIWYG, just add it to the right variable of the model.

:shared_medias_context: Common medias array.
:shared_wysiwygs_context: Common WYSIWYGs array.

Common medias and WYSIWYGs can be use like classic medias and WYSIWYGs, with accessors ``medias`` and ``wysiwygs`` of the model.

.. seealso:: :ref:`Accessors of Model <api:php/models/model/accessors>`.

Example
*******

.. code-block:: php

    <?php
    class Model_Monkey extends \Nos\Orm\Model
    {
        // ...

        public static $shared_medias_context = array(
            'thumbnail',
        );

        public static $shared_wysiwygs_context = array(
        );
    }
