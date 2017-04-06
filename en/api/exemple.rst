PHP sample script
=================

Analytics and message script
----------------------------

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

Autopromo banner script
-----------------------

Here is a sample code used to add an autopromo banner on your website.

Note that the script should be located in the destination `div`.

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
    $autopromoBannerDomain = $scriptElement['autopromo_banner_domain'];
    $autopromoBannerScript = $scriptElement['autopromo_banner_script'];
    $autopromoBannerCode = <<<EOS
        (function (a,d){var s,t,cs,ds,dd;s=d.createElement('script');cs=d.currentScript;
        ds=d.createElement('span');ds.id=Math.random().toString(36).substring(7);
        dd=cs.parentNode.insertBefore(ds,cs);
        s.src=a;s.async=1;s.setAttribute('data-name',ds.id);s.setAttribute('data-id','base64encodedBannerId');
        t=d.getElementsByTagName('script')[0];t.parentNode.insertBefore(s,t);})
        })("https://$autopromoBannerDomain/$autopromoBannerScript.js", document);
    EOS;


    /*
    DISPLAY SCRIPT
    */
    echo "<script>$autopromoBannerCode</script>";

In this sample script `base64encodedBanneId` represents the banner id encoded in base64, you will be able to find
it in the AdBack dashboard.
