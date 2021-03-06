.. _purge_logs:

**********
Purge Logs
**********

Keeping records of personal communications for long periods may be subject to local legislation, to
avoid personal data retention. Also, keeping too many records may become resource intensive for the
server. To ease the removal of such records, ``xivo-purge-db`` is a process that removes old log
entries from the database. This allows keeping records for a maximum period and deleting older ones.

By default, xivo-purge-db removes all logs older than a year (365 days). xivo-purge-db is run
nightly.

.. note:: Please check the laws applicable to your country and modify ``days_to_keep`` (see below)
          in the configuration file accordingly.


Tables Purged
-------------

The following features are impacted by xivo-purge-db:

- :ref:`call_logs`
- :ref:`Call center statistics <call_center_stats>`

More technically, the tables purged by ``xivo-purge-db`` are:

-  ``call_logs``
-  ``cel``
-  ``queue_log``
-  ``stat_agent_periodic``
-  ``stat_call_on_queue``
-  ``stat_queue_periodic``


.. _purge_logs_config_file:

Configuration File
------------------

We recommend to override the setting ``days_to_keep`` from ``/etc/xivo-purge-db/config.yml`` in a
new file in ``/etc/xivo-purge-db/conf.d/``.

.. warning:: Setting ``days_to_keep`` to 0 will NOT disable ``xivo-purge-db``, and will remove ALL
             logs from your system.

See :ref:`configuration-priority` and ``/etc/xivo-purge-db/config.yml`` for more details.


Manual Purge
------------

It is possible to purge logs manually. To do so, log on to the target XiVO server and run::

    xivo-purge-db

You can specify the number of days of logs to keep. For example, to purge entries older than 365
days::

    xivo-purge-db -d 365

Usage of ``xivo-purge-db``::

    usage: xivo-purge-db [-h] [-d DAYS_TO_KEEP]

    optional arguments:
      -h, --help            show this help message and exit
      -d DAYS_TO_KEEP, --days_to_keep DAYS_TO_KEEP
                            Number of days data will be kept in tables


Maintenance
-----------

After an execution of ``xivo-purge-db``, postgresql's `Autovacuum Daemon`_ should perform a
`VACUUM`_ ANALYZE automatically (after 1 minute). This command marks memory as reusable but does
not actually free disk space, which is fine if your disk is not getting full. In the case when
``xivo-purge-db`` hasn't run for a long time (e.g. upgrading to 15.11 or when
:ref:`days_to_keep <purge_logs_config_file>` is decreased), some administrator may want to perform
a `VACUUM`_ FULL to recover disk space.

.. warning:: VACUUM FULL will require a service interruption. This may take several hours depending
             on the size of purged database.
.. _VACUUM: http://www.postgresql.org/docs/9.1/static/sql-vacuum.html
.. _Autovacuum Daemon: http://www.postgresql.org/docs/9.1/static/routine-vacuuming.html#AUTOVACUUM

You need to::

   $ xivo-service stop
   $ sudo -u postgres psql asterisk -c "VACUUM (FULL)"
   $ xivo-service start


Archive Plugins
---------------

In the case you want to keep archives of the logs removed by xivo-purge-db, you may install plugins
to xivo-purge-db that will be run before the purge.

XiVO does not provide any archive plugin. You will need to develop plugins for your own need. If you
want to share your plugins, please open a `pull request`_.

.. _pull request: https://github.com/xivo-pbx/xivo-purge-db/pulls


Archive Plugins (for Developers)
---------------------------------

Each plugin is a Python callable (function or class constructor), that takes a dictionary of
configuration as argument. The keys of this dictionary are the keys taken from the configuration
file. This allows you to add plugin-specific configuration in ``/etc/xivo-purge-db/conf.d/``.

There is an example plugin in the `xivo-purge-db git repo`_.

.. _xivo-purge-db git repo: https://github.com/xivo-pbx/xivo-purge-db/tree/master/contribs


Example
*******

Archive name: sample

Purpose: demonstrate how to create your own archive plugin.


Activate Plugin
^^^^^^^^^^^^^^^

Each plugin needs to be explicitly enabled in the configuration of ``xivo-purge-db``. Here is an
example of file added in ``/etc/xivo-purge-db/conf.d/``:

.. code-block:: yaml
   :linenos:

   enabled_plugins:
       archives:
           - sample


sample.py
^^^^^^^^^

The following example will be save a file in ``/tmp/xivo_purge_db.sample`` with the following
content::

   Save tables before purge. 365 days to keep!

.. code-block:: python
   :linenos:

    sample_file = '/tmp/xivo_purge_db.sample'

   def sample_plugin(config):
       with open(sample_file, 'w') as output:
           output.write('Save tables before purge. {0} days to keep!'.format(config['days_to_keep']))


Install sample plugin
^^^^^^^^^^^^^^^^^^^^^

The following ``setup.py`` shows an example of a python library that adds a plugin to xivo-purge-db:

.. code-block:: python
   :linenos:
   :emphasize-lines: 15-17

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    from setuptools import setup
    from setuptools import find_packages


    setup(
        name='xivo-purge-db-sample-plugin',
        version='0.0.1',

        description='An example program',
        packages=find_packages(),
        entry_points={
            'xivo_purge_db.archives': [
                'sample = xivo_purge_db_sample.sample:sample_plugin',
            ],
        }
    )
