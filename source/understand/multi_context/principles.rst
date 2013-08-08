.. index:: Multi-Contexts

Multi-contexts basics
#####################

| Novius OS can natively manage several websites, and each of them can have several linguistic versions.
| A context is a site / language pair.

Example
*******

Your Novius OS instance can manage your showcase website, which exists in 3 languages (French, English and Spanish),
your mobile website, which only exists in French and your events website, which exists in English.

=============== ======== ======= ========
Site / Language French   English Spanish
=============== ======== ======= ========
Showcase        X        X       X
Mobile          X
Events          X
=============== ======== ======= ========

In this situation, yout Novius OS instance manages **5** contexts:

* Showcase / French
* Showcase / English
* Showcase / Spanish
* Mobile / French
* Events / English

Configuration
*************

Everything to configure the contexts is described in the :ref:`API documentation <api:php/configuration/software/multi_contexts>`

Special cases
*************

He who can do more can do less. Your Novius OS can manage:

* a single website with several languages;
* several websites in a single language;
* a single website in a single language.

| The back-office interface reacts to any of these situations. The _context_ word will disappear in favor of _language_
  or _site_.
| It can ever vanish completely for a single website in a single language.

Adding contexts
***************

It can be done at any time! Novius OS will happily manage every new contexts, sites or languages found in the configuration.
Just change the :file:`contexts.config.php` file and new contexts will be taken into account straight away.

.. seealso::

	:ref:`API documentation on multi-contexts <api:php/configuration/software/multi_contexts>`.
