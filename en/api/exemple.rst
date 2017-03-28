PHP sample script
=================

Here is a sample script for server to server API usage :

.. code-block:: php

    <?php

    /*
    CONNECT TO YOUR IN-MEMORY DATA STRUCTURE STORE LIKE REDIS
    */
    $cache = new Redis();
    $cache->connect('host');

    /*
    GET DATA FROM EITHER IN YOUR IN-MEMORY DATA STRUCTURE STORE OR THE API
    */
    if ($cache->has('scriptElement')) {
        $scriptElement = $cache->hGetAll('scriptElement');
    } else {
        $scriptElement = json_decode(file_get_contents('https://adback.co/api/script/me?
        access_token=token'), true);
        foreach ($scriptElement as $key => $value) {
            $cache->hSet('scriptElement', $key, $value);
        }
        $cache->expire('scriptElement', 60*60*24);
    }

    /*
    CREATE THE ANALYTICS SCRIPT
    */
    $analyticsDomain = $scriptElement['analytics_domain'];
    $analyticsScript = $scriptElement['analytics_script'];
    $analyticsScript = <<<EOS
        (function (a,d){var s,t;s=d.createElement('script');
        s.src=a;s.async=1;
        t=d.getElementsByTagName('script')[0];
        t.parentNode.insertBefore(s,t);
        })("https://$analyticsDomain/$analyticsScript.js", document);
    EOS;

    /*
    CREATE THE MESSAGE SCRIPT
    */
    $messageScript = '';
    if (isset($scriptElement['message_domain']) {
        $messageDomain = $scriptElement['message_domain'];
        $messageScript = $scriptElement['message_script'];
        $customMessageScript = <<<EOS
            (function (a,d){var s,t;s=d.createElement(‘script’);
            s.src=a;s.async=1;
            t=d.getElementsByTagName('script')[0];
            t.parentNode.insertBefore(s,t);
            })("https://$messageDomain/$messageScript.js", document);
    EOS;
    }

    /*
    DISPLAY BOTH SCRIPTS
    */
    echo "<script>$analyticsScript</script><script>$messageScript</script>";
