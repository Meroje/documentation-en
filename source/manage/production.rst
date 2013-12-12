Production
##########

From localhost to your production server
****************************************

You can send Novius OS on a production server using many way:

* The simplest way would be to copy all Novius OS files as well the database from your local machine to the server.
  However, as the data is generally different between these two instances, this is not very convenient. And you will
  probably have to change configuration files.
* You can send all files except those in `local/metadata` and `local/data` folders. You will need to install Novius OS
  on the production server, only for the first time. This way you will be able to easily configure mysql connection,
  urls and administration accounts.

However, regardless the method you choose, you will have to change few configuration settings to improve optimization.

Changing environment to production
**********************************

The first step is to change the Fuel environment (stored in `Fuel::$env`). This will automatically adapt few settings
such as cache length or logs level. The
`FuelPHP website <http://fuelphp.com/docs/general/environments.html#/env_apache>`__ explains how to change this
environment.

You can do it by changing `SetEnv` in the Apache configuration.

.. code-block:: apacheconf

    SetEnv FUEL_ENV production
    // or
    SetEnv NOS_ENV production

Database configuration
**********************

You need to add the `production` key into `local/config/db.config.php`. The configuration can be quite similar than the
one of `development`; if you installed the instance on the production server, you just have to rename the `development`
key to `production`. This is very well documented in the
`FuelPHP website <http://fuelphp.com/docs/classes/database/introduction.html>`__.

Customizing cache durations
***************************

Cache duration is adapted if the environment is set to production. You can however customize it by changing
`local/config/config.php` file.

.. code-block:: php

    return array(
        'novius-os' => array(
            'cache' => true,
             // When on production environment, durations are 3600 seconds by default
            'cache_duration_page' => 3600, // page cache duration
            'cache_duration_function' => 3600, // custom (applications) cache duration
            'cache_model_properties' => false, // does Novius OS store model properties into cache. Applies only to
            // models where properties where not defined
        ),
    );

Email configuration
*******************

If need your Novius OS instance to send emails, you have to rename the file `local/config/email.config.php.sample` to
`local/config/email.config.php`. Configuration details are very well explained on the
`FuelPHP website <http://fuelphp.com/docs/packages/email/introduction.html>`_.