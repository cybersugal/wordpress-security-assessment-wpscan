
# Raw WPScan Output

## Scan Command

```bash
wpscan --url http://192.168.1.71/wordpress-6.5-en_GB/wordpress/ --enumerate vp,vt,u
```

## Original Scanner Output

```text
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

        WordPress Security Scanner by the WPScan Team

                       Version 3.8.28
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://192.168.1.71/wordpress-6.5-en_GB/wordpress/ [192.168.1.71]
[+] Started: Thu Oct 1 05:21:37 2026

Interesting Finding(s):

[+] Headers
 | Interesting Entries:
 | - Server: Apache/2.4.58 (Win64) OpenSSL/3.1.3 PHP/8.2.12
 | - X-Powered-By: PHP/8.2.12
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://192.168.1.71/wordpress-6.5-en_GB/wordpress/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 | - http://codex.wordpress.org/XML-RPC_Pingback_API
 | - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 | - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 | - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 | - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://192.168.1.71/wordpress-6.5-en_GB/wordpress/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://192.168.1.71/wordpress-6.5-en_GB/wordpress/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://192.168.1.71/wordpress-6.5-en_GB/wordpress/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%

 | References:
 | - https://www.iplocation.net/defend-wordpress-from-ddos
 | - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 7.1.2 identified (Latest, released on 2026-09-22).
 | Found By: Meta Generator (Passive Detection)
 | - http://192.168.1.71/wordpress-6.5-en_GB/wordpress/, Match: 'WordPress 7.1.2'
 | Confirmed By: Rss Generator (Aggressive Detection)
 | - http://192.168.1.71/wordpress-6.5-en_GB/wordpress/feed/
 |   <generator>https://wordpress.org/?v=7.1.2</generator>
 | - http://192.168.1.71/wordpress-6.5-en_GB/wordpress/comments/feed/
 |   <generator>https://wordpress.org/?v=7.1.2</generator>

[i] The main theme could not be detected.

[+] Enumerating Vulnerable Plugins (via Passive Methods)
[i] No plugins Found.

[+] Enumerating Vulnerable Themes (via Passive and Aggressive Methods)
Checking Known Locations - Time: 00:02:47
<===================================================================>
<============================> (652 / 652) 100.00% Time: 00:02:47

[i] No themes Found.

[+] Enumerating Users (via Passive and Aggressive Methods)
Brute Forcing Author IDs - Time: 00:00:05
<===================================================================>
<==============================> (10 / 10) 100.00% Time: 00:00:05

[i] User(s) Identified:
[+] cyber_admin
 | Found By: Wp Json Api (Aggressive Detection)
 | - http://192.168.1.71/wordpress-6.5-en_GB/wordpress/wp-json/wp/v2/users/?per_page=100&page=1
 | Confirmed By:
 | - Rss Generator (Aggressive Detection)
 | - Author Id Brute Forcing - Author Pattern (Aggressive Detection)

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at
    https://wpscan.com/register

[+] Finished: Thu Oct 1 05:24:51 2026

[+] Requests Done: 699
[+] Cached Requests: 5
[+] Data Sent: 190.417 KB
[+] Data Received: 1002.97 KB
[+] Memory used: 294.168 MB
[+] Elapsed time: 00:03:14
```

## Notes

- The output above records the scan findings supplied for this project.
- WPScan reported XML-RPC availability, directory listing, possible external WP-Cron exposure, server information, and a discoverable username.
- No plugins or themes were found by the enumeration performed in this scan.
- No WPScan API token was supplied, so vulnerability database results were not included.
- These results reflect the scan at the recorded time and do not, by themselves, prove that a system was successfully exploited.

**Ethical note:** This scan targeted a local test environment. Only scan systems you own or have explicit permission to assess.
