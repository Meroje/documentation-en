.. index:: Media centre

Media centre
############

General informations
********************

The media centre is the central place which gather most of the files used by the applications. It contains images,
documents, videos or any other file.

* All files are store in the **private** directory :file:`/novius-os/local/data/media/`
* They are accessed using the URL :file:`http://your.website.com/{media}/folder/ressource.ext`


Functioning
***********

When accessing a media for the first time, the 404 handle is invoked. It creates a symbolic link in the
:file:`public/media` directory, so subsequent requests don't need to use the 404 handler anymore.

It works this way because we'll be able to handle **private** medias in the future. The latter will return:

* a HTTP 401 error code (authorization required);
* or the file will be sent on the standard output, but without creating a symbolic link (permissions need to be checked
  for each and every request).

Optimisation
************

When PHP sends a big file on the ouput, it blocks the process until the transfer is completely done. But it's possible
to release the process instantaneously by delegating the work to the underlying web server (usually Apache or nginx).

The `XSendfile <http://wiki.nginx.org/XSendfile>`__ mechanisme can be used. It consists of sending a special header from
the PHP script, its name can vary upon one server or another:

* ``X-Sendfile`` is used by **Apache** and some others ;
* ``X-Accel-Redirect`` is used by **nginx**.


.. seealso:: :doc:`/install/post_install_optimisations`

.. index:: Attached files
.. index:: Attachment

Attached files (outside the media centre)
*****************************************

You may not want to store all your files in the media centre. For instance, a 'human resources' application collects
CV of candidates, and you don't want them to be visible in the media centre.

There's an :doc:`associated file </app_create/attachment>` mechanism to handle this case. It works roughly the same as
email attachments and allows to store a CV with its candidates data.
