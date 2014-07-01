Add common fields to all contexts
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

For define a common media or WYSIWYG, use :ref:`providers <api:php/behaviours/twinnable/providers>` ``shared_wysiwygs_context`` and ``shared_medias_context`` added by the ``Twinnable`` behaviour.

