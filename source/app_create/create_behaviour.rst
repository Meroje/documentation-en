Create your own Behaviour
#########################


Why doing it?
=============

Creating a behaviour allows add a same set of functionalities among several models, by sharing the same reusable code base.

Behaviours are an extension to the Observers mechanism existing in FuelPHP. They allow (the same way as Observers do) to
listen the ``before_*`` and ``after_*`` notifications, triggered by FuelPHP.

They allow two more functionalities:

* listening to events triggered by Novius OS (especially by the AppDesk and the CRUD) ;
* adding dynamically additional methods on your models (in a reusable way).

Here are some examples to better understand:

* adding an action in the App Desk or the CRUD ;
* adding a column in the dataset (data_mapping) of a model ;
* adding a field in the add/edit form of an item ;
* adding a new ``myBehaviourMethod()`` method for all the models using this behaviour.

More concretely:

- The :ref:`Publishable <api:php/behaviours/publishable>` behaviour, ass a field in the CRUD configuration and displays it
  in the form byb using the Renderer_Publishable.
- The :ref:`Urlenhancer <api:php/behaviours/urlenhancer>`, :ref:`Twinnable <php/behaviours/twinnable>` and :ref:`Sharable <php/behaviours/sharable>`
  behaviours respectively adds the following actions: **visualise**, **translate** and **share**.


Extending the Orm_Behaviour class
=================================

To create a behaviour, you need to create a class that extends **Nos\\Orm_Behaviour**, and implementing the following:

* methods to listen to FuelPHP events (``before_*`` et ``after_*``), in the same manner as Observers ;
* methods to listen to Novius OS events (triggered by the App Desk and the CRUD) ;
* additional methods to add to all the models which are using your behaviour.

You can add any class as a behaviour by adding its full classname to the ``$_behaviours`` property of your model.

Properties
----------

Exactly like FuelPHP's observers, models can use a configuration array when defining their behaviours. This configuration
will be available in the **$this->_properties** variable from inside the behaviour.

.. code-block:: php

    <?php

    class Model_Monkey extends Nos\Orm\Model
    {
        protected static $_behaviours = array(
            'My_Behaviour' => array(
                'my_key' => 'my_value',
            ),
        );
    }

.. code-block:: php

    <?php

    class My_Behaviour extends Nos\Behaviour
    {
        /*
         * In any method, $this->_properties will contain :
         * array(
         *     'my_key' => 'my_value',
         * )
         */
    }


Listening to a FuelPHP event (Observer)
---------------------------------------

These are the ``before_*`` and ``after_*`` events.

For example, the ``Behaviour_Author`` stores the ID of the users who created / updated an item in a column of the model,
thanks to (respectively) the ``before_insert`` and ``before_save`` `events available in FuelPHP's ORM <http://fuelphp.com/docs/packages/orm/observers/creating.html#/event_names>`__.


.. code-block:: php

    <?php

    class Orm_Behaviour_Author extends Orm_Behaviour
    {
        public function before_insert(\Nos\Orm\Model $item)
        {
            $created_by_property = \Arr::get($this->_properties, 'created_by_property', null);
            if ($created_by_property === null) {
                return;
            }

            $user = \Session::user();
            if (!empty($user)) {
                $item->{$created_by_property} = $user->user_id;
            }
        }
    }



Listening to a Novius OS event
------------------------------

In the same manner as observers do, a method named after the triggered event must be implemented.

For example, to listen to a **form_processing** event, we need to implement a **form_processing()** method.

The difference with events triggered by FuelPHP lies in the parameters send to these methods:

Observers events (``before_*`` and ``after_*``) have an unique **$item** parameter (the model instance), whereas events
triggered by Novius OS can take several ones, depending on the event type.

Two types of events exists:

* instance events, which always receive the **$item** as a first parameter, and optionally other parameters specific to the event ;
* static events, which only receive parameters specific to the event.

The :ref:`list of available events (both instance and static) <api:php/behaviours/behaviour_event>` can be found in the API documentation.

An event is called on all Behaviour which implemented the corresponding method. The return value has no use : events use
`arguments passed by reference <http://php.net/manual/en/language.references.pass.php>` to do their job.


Exemple with the  ``form_processing`` **instance event** (triggered when an item is saved by the CRUD):

.. code-block:: php

    <?php

    class My_Behaviour extends Nos\Behaviour
    {
        public function form_processing(Nos\Orm\Model $item, $data, &$json_repsonse)
        {
            // Examples:
            // We fill in values to save in the item
            // We add some keys in the JSON array
        }
    }

    // For information: internally, Novius OS calls this event in the following manner:
    $item->event('form_processing', array($data, &$json_response));


Example with the ``crudConfig`` **static event**:

.. code-block:: php

    <?php

    class My_Behaviour extends Nos\Behaviour
    {
        public function crudConfig(&$config, $controller)
        {
            // Example:
            // We add a field by modifying $config['fields']
        }
    }

    // For information: internally, Novius OS canns this event in the following manner:
    Model_Class::eventStatic('crudConfig', $config, $controller);



Adding dynamically an instance method on a model
------------------------------------------------

Exactly the same as events triggered by FuelPHP and instance events, dynamics methods are named after the method to add
on the model, and take the **$item** as a first parameter (the model instance).

Unlike events, methods usually returns a value.

For example, the ``Behaviour_Contextable`` from Novius OS adds a ``get_context()`` methods on the model using it:

.. code-block:: php

    <?php

    // Model file
    class Model_Monkey extends Nos\Orm\Model
    {
        protected static $_behaviours = array(
            'Orm_Behaviour_Contextable' => array(
                'context_property' => 'monk_context',
            ),
        );
    }


    // Behaviour file
    class Orm_Behaviour_Contextable extends Nos\Behaviour
    {
        public function get_context(Orm\Model $item)
        {
            return $item->get($this->_properties['context_property']);
        }
    }

    // Use case
    $monkey = Model_Monkey::find('first');

    // This methods is available, because the Model_Monkey uses the Behaviour_Contextable, which makes it available
    $context = $monkey->get_context();


Adding dynamically a static method on a model
---------------------------------------------

It's the same as an instance method, but without the first **$item** parameter.

.. code-block:: php

    <?php

    // Model file
    class Model_Monkey extends Nos\Orm\Model
    {
        protected static $_behaviours = array(
            'Orm_Behaviour_Twinnable' => array(
                'context_property'      => 'monk_context',
                'common_id_property' => 'monk_context_common_id',
                'is_main_property' => 'monk_context_is_main',
                'common_fields'   => array('monk_species_common_id', 'monk_birth_year'),
            ),
        );
    }


    // Behaviour file
    class Orm_Behaviour_Twinnable extends Nos\Behaviour
    {
        public function hasCommonFields()
        {
            $class = $this->_class;
            return count($this->_properties['common_fields']) > 0 ||
                static::sharedWysiwygsContext($class) > 0 ||
                static::sharedMediaContext($class) > 0;
        }
    }

    // Use case
    Model_Monkey::hasCommonFields();

