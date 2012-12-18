Coding standards
=====================================

These standards for code formatting must be followed by anyone contributing to Novius OS.

You can use the `ruleset.xml for PHP_CodeSniffer <https://github.com/novius-os/ci/blob/dev/phpcs/ruleset.xml>`_ made for Novius OS.

Case
----

All keywords are in lowercase (class, interface, extends, implements, abstract, final, var, const, function, public, private, protected, static, if, else, elseif, foreach, for, do, switch, while, try, catch, true, false and null).

All constant names are in uppercase (global or class constant).

Signature of control statements
-------------------------------

.. code-block:: php

	try {
	    ...
	} catch (...) {
	    ...
	}

	do {
	    ...
	} while (...);

	while (...) {
	    ...
	}

	// A single space between each condition in 'for' loops.
	for ($i = 0; $i < 10; $i++) {
	    ...
	}

	// A single space between each condition in 'foreach' loops.
	foreach ($array as $key => $item) {
	    ...
	}

	if (...) {
	    ...
	} else if (...) {
	    ...
	} else {
	    ...
	}

	// A single space after cast tokens.
	$variable = (array) $variable;


Function declaration
--------------------

.. code-block:: php

	/* 
	The opening brace of a function is on the line after the function declaration.
	No space before comma.
	A single space after comma.
	A single space between type hint and argument.
	A single space before '=' sign of default value.
	A single space after '=' sign of default value.
	Parameters with a default value come at the end of the function signature.
	*/
	function myfunction(array $first, $second, $third = array())
	{
		...
	}

Function call
-------------

.. code-block:: php

	/* 
	No space before comma.
	A single space after comma.
	*/
	$value = myfunction($first, $second, $third);


Class
-----

.. code-block:: php

	/* 
	The opening brace of a class must be on the line after the definition.
	There must be one blank line after the namespace declaration.
	All class members have scope modifiers (variables, functions and methods).
	A single space after scope keywords (private, public, protected, static).
	*/
	namespace Name\Space;

	class MyClass
	{
		private $variable1 = null;

		protected static $variable1 = true;

		const MY_CONSTANT = false;

		public static function myfunction() 
		{
			...
		}
	}

File
----

End of line characters must be \n.
Files do not end with a closing tag.

Only one statement by line.

PHP code must use the long <?php ?> tags or the short-echo <?= ?> tags; it must not use the other tag variations.

Closing braces of scopes correctly aligned.
Control structures are correctly indented (4 spaces, no tab).

No additional white space at start and end of file.
