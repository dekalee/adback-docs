Usage
=====

To update your local cache, run the command:

.. code-block:: php
    php app/console dekalee:adback-anayltics:refresh-tag

To display the tag in your page, you can add the twig command:

.. code-block:: twig
    {{ adback_generate_scripts() }}
