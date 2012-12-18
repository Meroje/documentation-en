JavaScript API
==============

Summary
-------

* :ref:`javascript_api_notifications`
* :ref:`javascript_api_tabs`
* :ref:`javascript_api_ajax`
* :ref:`javascript_api_dialog`
* :ref:`javascript_api_events`
* :ref:`javascript_api_forms`
* :ref:`javascript_api_onshow`


.. _javascript_api_notifications:

Notifications
-------------

Wrapper around the jQuery `Pines Notify <http://pinesframework.org/pnotify/>`_ plugin.

Usage
^^^^^

``$.nosNotify(message, type);``

Examples
^^^^^^^^

.. code-block:: js

	// Simple notification
	$.nosNotify('Hello notification');

	// Error notification
	$.nosNotify('Error message', 'error');


.. _javascript_api_tabs:

Tabs
----

Usage
^^^^^

``$(context).nosTabs(action, params)``

1. add / open
^^^^^^^^^^^^^

``add`` will always create a new tab.
``open`` will either create a new tab, or refocus an existing tab with the same URL.

The icon size is 16*16px. It can be provided either as:

* an image URL
* CSS classes (background-image / sprite)

Options
"""""""

.. code-block:: js

	{
		url: '',
		iframe: false,
		label: '',
		iconClasses: 'ui-icon ui-icon-document',
		iconUrl: ''
	}


If the URL content returns a valid JSON value, it will be parsed by our native Ajax response handler rather than opening a new tab.

2. close
^^^^^^^^

Closes the current tab.

.. code-block:: js

	$(context).nosTabs('close');

3. update
^^^^^^^^^

Update tab informations (URL, label and icon, see ``options`` from ``add / open``).

If an URL is provided, the tab won't be reloaded (replaced) unless you set the ``reload: true`` option.

Options
"""""""

* All options from ``add`` / ``open``
* ``reload``: false

Example
"""""""

.. code-block:: js

	$(context).nosTabs('open', {
		'url' => 'admin/nos/page/page/insert_update/2',
		'label' => "Page's title",
		'iconUrl' => 'static/novius-os/admin/novius-os/img/16/page.png',
	});

.. _javascript_api_ajax:

Ajax
----

Usage
^^^^^

``$(context).nosAjax(params);``

The back-office is a big HTML page. Most requests are made using Ajax. Hence Novius OS has is wrapper around jQuery.

The ``nosAjax`` function has the same API as ``$.ajax``. There are two main differences:

* Default options
* Native operations performed on returned JSON

Default options
^^^^^^^^^^^^^^^

.. code-block:: js

	{
		dataType: 'json',
		type: 'POST'
	}


Callbacks & return value (JSON)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The ``success`` and ``error`` callbacks are `monkey-patched <http://en.wikipedia.org/wiki/Monkey_patch>`_ to perform default native operations (in addition to the callback provided by the user).

The returned JSON support the following actions:

* ``notify [string or array]`` : displays one or several notification(s).
* ``error`` [string or array]: displays one or several error notification(s).
* ``closeDialog [bool]``: used for ajax requests performed from a dialog.
* ``closeTab [bool]``: closes the current tab.
* ``replaceTab [string]``: URL of the new tab.
* ``dispatchEvent [object or array]``: see ``listenEvent()`` / ``dispatchEvent()``

Example of request
^^^^^^^^^^^^^^^^^^

.. code-block:: js

	// Performs a POST request, expecting JSON response
	$.nosAjax({
		'url': 'admin/nos/page/appdesk/clear_cache'
	});


Example of returned JSON
""""""""""""""""""""""""

.. code-block:: js

	{
		// This will trigger a notification
		'notify': 'Cache has been renewed.'
	}



.. _javascript_api_dialog:

Dialog
------

This API can be used to create a dialog:

* From an exising ``<div>``
* From an URL (``<iframe>``)
* From Ajax-fetched content

Default options
^^^^^^^^^^^^^^^

.. code-block:: js

	{
		destroyOnClose : true,
		width: window.innerWidth - 200,
		height: window.innerHeight - 100,
		modal: true,
		title: ''
	}

Usage
^^^^^

.. code-block:: js

	// Transform an existing div
	$(div).nosDialog();

	// Open an iframe
	$(context).nosDialog({
		contentUrl: 'http://...'
	});

	// Load Ajax HTML into the DOM and transform it into a dialog
	$(context).nosDialog({
		contentUrl: 'http://...',
		ajax: true,
		// optional data
		ajaxData: {} 
	});


If the Ajax URL returns a valid JSON value, it will be parsed by our native Ajax response handler rather than opening a new tab.


The dialog knows the tab context it's opened from. If you close a tab, any dialog contained within it will appropriately be destoyed.



.. _javascript_api_events:

Events
------

Some UI widgets are listening to particular events. For example, the appdesk's grid and inspectors are listening to events related to Model they display.

Events are bound to the tab they're being registered from. Events can be dispatched (created) anytime from any tab. Callbacks registered in the active (visible) tab will be triggered instantaneously. Callback registered in non-active tabs will be trigger when it become visible.


Standard event structure used in Novius OS
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: js

	{
		name: '', // model class related to the event. Example: ``Nos\Model_Page``
		id: 1, // primary key (or array) of the model
		action: '', // action performed. Examples: ``insert``, ``update``, ``delete``
		lang: '' // lang (or array) of the model
	}


Dispatched (created) events usually have a lot of informations than callback listen to. Listeners don't need the whole information to be useful, they can listen to a subset of the full event.


Listening
^^^^^^^^^

.. code-block:: js

	// Page grid listens the following events (if the selected lang is en_GB):
	$(context).nosListenEvent([
		{
			name: 'Nos\Model_Page',
			action: ['insert', 'delete']
		},
		{
			name; 'Nos\Model_Page',
			lang; 'en_GB'
		}
	], function() {
		// Reload the grid data
	});


The context will be listening to:

* all page's creation (from any language) or deletion (any page ID) ;
* any action performed on a page in en_GB (creation, deletion, update).

Both objects will match for the event ``{name:'Nos\Model_Page', action:'delete', lang: 'en_GB'}``.


Dispatching
^^^^^^^^^^^

This function does not require a context.

.. code-block:: js

	// This is triggered when a page is inserted
	$.nosDispatchEvent({
		name: 'Nos\Model_Page',
		action: 'insert',
		id: 4,
		lang: 'en_GB',
	});

	// This is triggered when a sub-tree is deleted
	$.nosDispatchEvent({
		name: 'Nos\Model_Page',
		action: 'delete',
		id: [4, 5, 9, 8],
		lang: ['en_GB', 'fr_FR'],
	});


.. _javascript_api_forms:

Forms
-----

UI enhancements
^^^^^^^^^^^^^^^

``$(context).nosFormUI();`` will perform UI enhancements on the form inputs. We rely mainly on `Wijmo <http://wijmo.com/wiki/index.php/Main_Page>`_, which use `jQuery UI <http://jqueryui.com/themeroller/>`_ for the theming:

* ``wijtextbox()``
* ``wijdropdown()``
* ``wijcheckbox()``
* ``wijradio()``
* ``wijexpander()`` for ``.expander`` elements
* ``wijaccordion()`` for ``.accordion`` elements

``<input type="submit">`` and ``<button>`` can have a ``data-icon="{icon-name}"`` or ``data-iconUrl="http://..."`` property. ``{icon-name}"`` is a standard icon name from jQuery UI.


Ajax
^^^^

``$(context).nosFormAjax();`` will use the `jquery-form <http://malsup.com/jquery/form/>`_ plugin to submit the form using Ajax rather than the native browser action. If there's validation too, it will run before submitting.

Most forms are using it, since it's part of the standard form layout.

Validation
^^^^^^^^^^

``$(context).nosFormValidate(params);`` will use the `jquery-validation <http://docs.jquery.com/Plugins/Validation>`_ plugin to perform inline validation on the elements. It's already configured to display error messages nicely, and take into account some specificity from the UI enhancements (like the accordion).

Most forms are using it, since it's part of the standard form layout.

.. _javascript_api_onshow:

On Show
-------

``nosOnShow()`` is a special API which delays the rendering of UI elements when they are visible.

A lot of UI elements don't initialise correctly when they are hidden (they can't calculate sizes properly).

.. code-block:: js

	// Instead of this:
	$(element).widget();

	// Do this:
	$(element).nosOnShow('one', function() {
		$(this).widget();
	});

	// You can also use 'bind' instead of 'one': the callback will be executed each time the element is shown, instead of just the first time.


This way, the element won't be initialised until it becomes visible. If you're responsible for the code displaying an element, then you just need to call ``.nosOnShow()`` to trigger the delayed actions on the children.

.. code-block:: js

	// Instead of this:
	$(element).show();

	// Do this:
	$(element).show().nosOnShow();


