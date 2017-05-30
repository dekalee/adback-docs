Configuration
=============

Base parameters
---------------

.. code-block:: yaml

    dekalee_adback_analytics:
        access_token: your-token
        cache_service: your.redis.service
        cache_type: [redis, doctrine, config_file]
        entity_manager: your-entity-manager

Getting your token
------------------

You can generate a token on the AdBack website :

1. Log in
2. Go on the `Api Access`_ page
3. Generate a token for the api client linked to your website

Cache type
----------

To avoid any slow down of your application, the bundle uses some local cache to store the gathered informations.

The bundle already provides a driver for ``Redis``, ``Doctrine``, and ``ConfigFile``.

Redis
~~~~~

You need to specify your redis service to make it work directly.

Doctrine
~~~~~~~~

You need to specify your entity manager and also create the table linked to the
``Dekalee\AdbackAnalyticsBundle\Entity\ApiCache`` entity.

ConfigFile
~~~~~~~~~~

You don't need to specify any further configuration, but not that the api is going to be called each time
you warmup your cache.
If you are using multiple servers, you need to run the update command on each of them to update the file cache.

Other
~~~~~

If you want to use another local cache service, you could check `how to implement a new driver`_.

Recommendation
--------------

According to your project size and type, we would recommand different cache types :

+--------------+---------------+----------------------+
| Project type | Single server | Multiple server      |
+==============+===============+======================+
| Symfony      | Config File   | Redis or Doctrine    |
+--------------+---------------+----------------------+
| EzPublish    | Config File   | Redis or Doctrine    |
+--------------+---------------+----------------------+
| Raw PHP      | MySqli / PDO  | Redis / MySqli / PDO |
+--------------+---------------+----------------------+

.. _`Api Access`: https://www.adback.co/fr/admin/api/
.. _how to implement a new driver: cache_storage.html
