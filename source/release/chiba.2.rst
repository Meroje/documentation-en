Release notes Chiba 2
#####################

New features
============

* Windows support (Vista and upper).
* Better install wizard (UI, more tests, choose of languages)
* Advanced permissions for all natives applications.
* Comments application:

    * Administration interface
    * Emails are sent when new comments are posted, to post author and others commenters.

Developer
=========

Breaking changes
----------------

* **Model**: If a column of a ``dataset`` contains HTML, you must also set :ref:`'isSafeHtml' => true <api:php/configuration/application/common>` if you don't want that it to be encoded (for security reasons).
* **CRUD**: For item updating, the callback function ``success`` is called after ``save`` (not before), like for creating.
* **Attachment**: Methods ``->url()`` and ``->urlResized()`` :ref:`return absolute URLs <api:php/classes/attachment>`. They accept an optional parameter for the return of relative URLs.
* **Comments**: Comments are now contextable. Migration tries to guess context of existing comments, but if you've implement comments on a not contextable model, migration can't do nothing: set yourself context if you want to see those comments in new administration interface.
* **Blog/News**: Thumbnail is now configurable (size & link).

    * Default thumbnail size changed from 200 everywhere to 120 in the listing and 200 on the item.
    * Thumbnail can now be clicked to go on the item page (set ``thumbnail.front.list.link_to_item = false`` to restore old behaviour).

Vendors update
--------------

* FuelPHP 1.6
* jQuery 1.9.1
* jQuery UI 1.10.3
* Wijmo 2013v1.4
* require.js 2.1.6

Improvements
------------

* **i18n**: :doc:`Default dictionary </app_create/translate>` ``app::default`` is used if no dictionary is set with ``Nos\I18n::current_dictionary()``.
* **DB**: Change interclassement on all columns use like a slug.
* **ORM**: Refactoring ``behaviour`` implementation (:ref:`behaviours can intercept model events <api:php/behaviours/behaviour_event>`).
* **ORM**: Improvement of the model properties' cache mechanism, just one request of ``columns`` from DB by thread.
* **ORM**: 4 new relation types, :ref:`twinnable_belongs_to <api:php/relations/twinnable_belongs_to>`, :ref:`twinnable_has_one <api:php/relations/twinnable_has_one>`, :ref:`twinnable_has_many <api:php/relations/twinnable_has_many>`, :ref:`twinnable_many_many <api:php/relations/twinnable_many_many>`.
* **ORM**: Model class, new ``addRelation()``, ``configModel()``, ``getApplication()`` methods.
* **Behaviour Twinnable**: Models now can have :ref:`fields <api:php/behaviours/twinnable/configuration>`, :ref:`medias and WYSIWYGs <api:php/models/model/configuration>` common to all contexts.
* **Behaviour Twinnable**: new ``findMainOrContext()``, ``hasCommonFields()``, ``isCommonField()`` :ref:`methods <api:php/behaviours/twinnable/methods>`.
* **Behaviour URLEnhancer**: New :ref:`methods <api:php/behaviours/urlenhancer/methods>` ``deleteCacheEnhancer()`` and ``deleteCacheItem()``.
* **Behaviour URLEnhancer**: Delete front's cache of the item on deleting and updating.
* **Behaviour**: New :ref:`behaviour author <api:php/behaviours/author>`, use by Page, Media, Blog/News, Slideshow, Form
* **Enhancer**: In popup configuration, new ability to define a ``layout`` and ``fields`` :doc:`configuration </app_create/enhancer>` instead of a view, much like the CRUD.
* **Enhancer**: In :ref:`enhancer configuration <api:metadata/enhancers>`, new possible key ``valid_container``, which is callable. Can restrict the enhancer availability depending on container.
* **Enhancer**: In front displaying, output is wrap in a ``div`` with classes ``noviusos_enhancer`` and the enhancer name (``noviusos_blog``, ``noviusos_news``, ``noviusos_slideshow``, ``noviusos_form``)
* **Renderer**: Added a :ref:`datetime picker <api:php/renderers/datetime>` renderer to manage both date and time in the same input.
* **WYSIWYG**: :ref:`new WYSIWYG configuration mechanism <api:php/configuration/wysiwyg>`, with a ``wysiwygOptions`` event registrable by behaviour (and use by twinnable), and ``wysiwyg`` config sample file.
* **WYSIWYG**: In ``Nos::parse_wysiwyg()``, replacing anchors by ``URL#anchor`` only in front.
* **SEO**: :ref:`new friendly slug configuration mechanism <api:php/configuration/friendly_slug>`, with a ``friendlySlug`` event registrable by behaviour (and use by twinnable), and ``friendly_slug`` config sample file.
* **OsTabs**: :ref:`new reload method <api:javascript/$container/nosTabs>` in API
* **OsTabs**: Change in tabs opening position. Tab added without index now is added at ``selected + 1``, except when the selected is desktop, adds the new tab at the end.
* **Appdesk**: Two new keys, ``css`` and :ref:`notify <api:php/configuration/application/appdesk/notify>` in :ref:`appdesk configuration <api:php/configuration/application/appdesk>`.
* **Appdesk**: Ability to ignore a :ref:`cellFormatter <api:php/configuration/application/cellFormatters>` based on a column value.
* **Appdesk**: Now :ref:`custom cellFormatters <api:php/configuration/application/cellFormatters/custom>` are allowed in appdesks.
* **Grid**: new ``align`` key on :ref:`actions configuration <api:php/configuration/application/common/actions>`.
* **Grid**: new option for the :ref:`initial opening depth <api:php/configuration/application/appdesk/appdesk>` on tree grid.
* **UI**: Using ``.ui-priority-primary`` instead ``.primary`` on button and ``.title`` on textbox inputs.
* **UI**: Use browser native select, checkbox and radio, no more use of Wijmo widget for those inputs.
* **Page**: Set home page not allowed in multi-context view.
* **Page**: Delete or unpublish the home page is not allowed.
* **Page**: Increased title and url fields size.
* **Media**: New field ``filesize``. Display ``filesize`` and dimensions in appdesk preview and CRUD form.
* **Media**: Refactoring ``get_img_tag()`` and ``get_img_tag_resized()`` methods of :ref:`Model_Media <api:php/models/media/model_media/methods>`, uses ``HTML::img()`` for returning a tag with attributs.
* **Media**: You can now transform (crop, rotate, rounded, watermark, resize, shrink, grayscale, border) Media and Attachment images by an :ref:`Toolkit_Image API <api:php/classes/toolkit_image>`.
* **Media**: New "Renew media's cache" action in Media appdesk toolbar, visible for expert users.
* **Media**: Increased title and url fields size.
* **Comments**: New API for use of ``noviusos_comments`` application.
* **Form**: New ``message`` view for the confirmation.
* **Misc**: New events :ref:`404.mediaFound <api:php/events/404.mediaFound>`, :ref:`404.attachmentFound <api:php/events/404.attachmentFound>`, :ref:`admin.loginFail <api:php/events/admin.loginFail>` and :ref:`nos.deprecated <api:php/events/nos.deprecated>`.
* **Misc**: All URLs are now urlencoded when use in a href or in a redirection.
* **Misc**: New ``temp`` directory in :file:`local/data`, assign to :ref:`novius-os.temp_dir <api:php/configuration/software>` config key by default.
* **Front**: ``is_preview`` is true only when you are logged in.

Deprecated
----------

* **Enhancer**: ``get_url_model($item, $params)`` in :ref:`enhancer front controller <app_create/enhancer/url>` is deprecated, please use ``getURLEnhanced($params)`` and ``$item`` in a key ``item`` of ``$params``.
* **Media**: Change :ref:`Model_Media API <api:php/models/media/model_media/methods>`, deprecating all snake_case methods.
* **Media**: Deprecating ``delete_from_disk()`` and ``delete_public_cache()`` :ref:`methods of Model_Folder <api:php/models/media/model_folder/methods>`. Use ``deleteFromDisk()`` and ``deleteCache()`` instead.
* **Page**: ``Model_Page->link()`` is deprecated, please use :ref:`Model_Page->htmlAnchor() <api:php/models/model_page/methods>` instead.
* **Misc**: Event ``user_login`` is deprecated, please use :ref:`admin.loginSuccess <api:php/events/admin.loginSuccess>` instead.
