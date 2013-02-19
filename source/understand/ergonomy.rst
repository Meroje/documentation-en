Ergonomic guidelines
====================

Novius OS interface is built upon great ergonomic guidelines. To develop applications, two of them should be known:
tab navigation and App desk.

.. index:: Tabs

Tabs navigation
---------------

`See the screencast related to tabs navigation <http://youtu.be/0fbSDqVI6zc>`__

Tabs navigation organise the work of the end-user. The purpose is to increase productivity, by limiting repetitive
tasks and pages loading.

There are two types of tabs:

- **Application tab** : Application tabs doesn't have a title, but only a big icon of the application. It contains the
  App Desk (see below).
- **Item tab** : from the application tab, we reach the edition of visualisation of an item, which takes place in a new
  tab. These kind of tab shows the item's title and a small icon of the application.

.. image:: images/ergonomie-tabs.png
	:alt: Tabs navigation
	:align: center

There are many advantages to tab navigations. We'll emphasise:
- several items of the same application can be edited in parallel;
- extremely fast switching between different items;
- the user can resume its work where he left out (opened tabs are preserved upon time).

**pop-ups** must be limited to modal use, i.e, when an action must absolutely be accomplished (or canceled) before
carrying out the work (such as confirming a suppression, adding a link or an image to a WYSIWYG content).

.. _understand/ergnonomie/app_desk:


The App Desk
------------

`See the screencast related to the App Desk <http://youtu.be/JskI5qWEsHw>`__

The App Desk is the home page of an application, it allows to browse and access the items. It's made of the following
elements:

- **Main grid**: list the items of an application, one or severals views can be chosen (thumbnails, grid, tree, etc.).
  Its content is filtered by the inspectors and / or a full-text search. There's only one main grid for each App Desk.
- **Inspectors**: they constitute the meta items of an application (such as blog authors, or media folders). Inspectors
  allows to filter the content displayed in the main grid (to show the posts of a particular user). Some inspectors allows
  to handle the data too (for instance, deleting a folder).

	* Preview inspector: the preview inspector is a special case. Unlike others inspectors, it doesn't act on the main
	  grid, but rather the main grid acts on it: when an item is selected, details are shown in the preview inspector
	  (image, properties, summary of the possible actions on the item).

- **Actions**: in the vast majority of times, each App Desk must offer one and only one main action, which is
  :guilabel:`Add a new item`. Secondary actions can also appear as links: adding a meta item (like a folder) or other
  usual actions (such as exporting data).

.. image:: images/ergonomie-app-desk.png
	:alt: App Desk
	:align: center

The App Desk offers a lot of possible page layouts for developers and end-users. Meanwhile, we recommend to show a
`Three-Pane Interface <http://en.wikipedia.org/wiki/Three-pane_interface>`__ as default.

.. image:: images/ergonomie-tpi-fr.png
	:alt: Three-Pane Interface
	:align: center

Alternatively, the preview inspector can be placed under the main grid.