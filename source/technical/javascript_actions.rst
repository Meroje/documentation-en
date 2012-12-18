JavaScript actions
==================

Our JavaScript API provides a centralised way to execute actions without closures.

Those actions are defined only using plain JSON objects (or PHP arrays).

The ``$(context).nosAction()`` currently works for following actions:

* :ref:`javascript_actions_tabs`
* :ref:`javascript_actions_ajax`
* :ref:`javascript_actions_window-open`

Generic syntax
--------------

.. code-block:: js

	$(context).nosAction({
		action: 'actionName'
		// other keys depending on the action
	}, data);


``data`` is a JSON object used to replace ``{{placeholders}}`` in the first parameter.

Placeholders
^^^^^^^^^^^^

.. code-block:: js

	// Define an action using placeholders
	var myActionWithPlaceholders = {
		action: 'nosTabs',
		tab: {
			url: 'admin/nos/page/page/insert_update/{{id}}',
			label: '{{title}}',
			iconUrl => 'static/novius-os/admin/novius-os/img/16/page.png'
		}
	};

	// Define data to fill-in the placehoders
	var page = {
		id: 2,
		title: 'Homepage'
	};

	// Call the action, {{placeholders}} will be replaced before execution
	$(context).nosAction(myActionWithPlaceholders, page);


	// This is equivalent
	var myActionNoPlaceholders = {
		action: 'nosTabs',
		tab: {
			url: 'admin/nos/page/page/insert_update/2',
			label: 'Homepage',
			iconUrl => 'static/novius-os/admin/novius-os/img/16/page.png'
		}
	};
	$(context).nosAction(myActionNoPlaceholders);



.. _javascript_actions_tabs:

nosTabs (wrapper)
-----------------

:ref:`Documentation for nosTabs() <javascript_api_tabs>`

Example
^^^^^^^

.. code-block:: js

	// 1. Define a JSON action
	var myAction = {
		action: 'nosTabs',
		params: {}, // Params, as defined in the nosTabs() API
		method: '' // Optional. Examples: 'add', 'open', 'update', 'close'
	};

	// 2. Called it using the nosAction() wrapper
	$(context).nosAction(myAction);

	// The above is equivalent to
	$(context).nosTabs(myAction.method || 'open', myAction.params); 


.. _javascript_actions_ajax:

nosAjax (wrapper)
-----------------

:ref:`Documentation for nosAjax() <javascript_api_ajax>`

Example
^^^^^^^

.. code-block:: js

	// 1. Define a JSON action
	var myAction = {
		action: 'nosAjax',
		params: {} // Params, as defined in the nosAjax() API
	};

	// 2. Called it using the nosAction() wrapper
	$(context).nosAction(myAction);

	// The above is equivalent to
	$(context).nosAjax(myAction.params); 



.. _javascript_actions_window-open:

window.open
-----------

Example
^^^^^^^

.. code-block:: js

	// 1. Define a JSON action
	var myAction = {
		action: 'window.open',
		url: 'http://...'
	};

	// 2. Called it using the nosAction() wrapper
	$(context).nosAction(myAction);

	// The above is equivalent to
	window.open(myAction.url); 





