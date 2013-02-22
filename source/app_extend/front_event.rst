Alter a behaviour of the front-office
#####################################


Novius OS provides an event-base mechanisme to interact with the core.

There are 2 types of events:

- Those which can alter data ;
- Those which only notify an action occurred.

.. code-block:: php

	<?php

	// Exemple of notification event
	Event::register('event_name', function($value)
	{
	    // L'action 'event_name' s'est produite
	});


.. code-block:: php

	<?php

	// Exemple of an event which may alter data
	Event::register_function('event_name', function(&$value)
	{
	    // $value peut être modifiée
	});


All events are documented in the API.

.. seealso::

    :ref:`api:php/events`



Redirecting depending on the URL
********************************


It's possible to create an :guilabel:`External  link` page from the back-office to make a 301-redirect.

It's also possible to configure 301-redirects using the :file:`./htaccess` file.

Last, it's also possible to do it from the source code, like below:

.. code-block:: php

	<?php

	Event::register_function('front.start', function($params)
	{
	    // The Str class is from FuelPHP
	    if (Str::starts_with($params['url'], 'an-old-url'))
	    {
	        // Note: 10 == strlen('an-old-url')
	        $new_url = 'my-new-url'.substr($params['url'], 10);

	        // The Response class is from FuelPHP
	        Response::redirect($new_url, 'location', 301);
	    }
	});


Sending a thank-you mail from a contact form
********************************************

.. code-block:: php

	<?php

	Event::register_function('noviusos_form::after_submission', function(&$answer, $enhancer_args)
	{
	    foreach ($answer->fields as $field)
	    {
	        if ($field->anfi_field_type == 'email' && !empty($field->anfi_value)
	        {
	            $email = Email::forge();
                $email->from('my@email.me', 'My email');
                $email->to($field->anfi_value);
                $email->subject('Your contact request');

                // Textual email (use html_body() instead if you want to send HTML email)
                $email->body('Thank you for contacting us. We received it and will answer to you soon.');

                try
                {
                    $email->send();
                }
                catch(\Exception $e)
                {
                    // Could not send the email
                }
	        }
	    }
	});

