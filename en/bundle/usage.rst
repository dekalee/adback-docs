Usage
=====

Refresh cache
-------------

To update your local cache, run the command:

.. code-block:: php

    php app/console dekalee:adback-anayltics:refresh-tag

Add analytics and message tag
-----------------------------

To display the tag in your page, you can add the twig command:

.. code-block:: twig

    {{ adback_generate_scripts() }}

Add autopromo banner tag
------------------------

To display the autopromo banner in your page, you can add the twig command:

..code-block:: twig

    {{ adback_generate_autopromo_banner_script(bannerId) }}
