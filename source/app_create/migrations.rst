Migrations files
################

Each application as well as the local folder can have a `migrations` folder.
This folder contains migrations files that are a convenient way to update the database or files. The
`FuelPHP migration system <http://fuelphp.com/docs/general/migrations.html>`__ is used. However on Novius OS there are
two things you should know:

* Application migration files must be on the namespace `{{APPLICATION_NAMESPACE}}\\Migrations`
* Whenever it is possible, sql update must be on a separate file (in order to make easier manual updates). As most of
  the time only sql requests are executed, a migration class has been implemented in order to ease migrations. You can
  take a look at the :ref:`API documentation <api:php/classes/migration>`.

When an application is installed or updated, migration files are executed (if they haven't been already) through the
application manager.