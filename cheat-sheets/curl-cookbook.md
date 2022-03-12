
Last updated: 7 June 2020  
[Get coronavirus/Covid-19 statistics for your country, real-time or historical](#ee19)  
[Force curl not to show the progress bar](#ee0)  
[Download a web page via GET request setting Chrome version 74 as the User-Agent.](#ee1)   
[Download a web page via GET request setting Googlebot version 2.1 as the User-Agent](#ee2)   
[Download a page via https ignoring ceritficate errors](#ee3)   
[Download a page using SOCKS5 proxy listening on 127.0.0.1  port 10443](#ee4)   
[Download a page using SOCKS5 proxy listening on 127.0.0.1 port 10443 and use remote host to resolve the hostname](#ee5)   
[Download a page and report time spent in every step starting with resolving](#ee6)     
[Resolve IP address to the owner's Autonomous System Number by sending POST query with form fields to the Team Cymru whois server](#ee27)  
[Make sure Curl follows redirections (`Location:`) automatically, also using the correct `Referer` on each redirection](#ee7)   
[Send GET request with digest authentication](#ee8)   
[Download a remote file only if it's newer than the local copy](#ee9)   
[Enable support for compressed encoding in response, as the real browser would do](#ee10)    
[Verify CORS functionality of a website](#ee11)   
[Convert Curl command into ready to be compiled C source  file](#ee12)   
[Display just the HTTP response code](#ee13)   
[Get a page using specific version of HTTP protocol](#ee25)     
[Download file with SCP protocol](#ee14)    
[Get external IP address of the machine where the curl is installed](#ee15)  
[Send e-mail via SMTP](#ee16)      
[Make curl resolve a hostname to the custom IP address you specify without modifying hosts file or using DNS server hacks](#ee17)  
[Show how many redirects were followed fetching the URL](#ee18)    
[Use your browser to prepare the complete curl command via "copy as curl" feature](#ee20)  
[Test if a website supports the given cipher suite, e.g. obsolete sslv3](#ee21)  
[Fetch multiple pages with predictable pattern in their URLs](#ee22)  
[How to prevent errors on URLs that contain brackets](#ee23)  
[Github: list names of all public repositories for a given user](#ee24)  
[Display weather report for a given city](#ee26)  

 


<a name="ee19"></a>  
### Get coronavirus/Covid-19 statistics for your country, real-time or historical
Add your country code after the slash, e.g. for Israel "il".  
```Bash
$ curl -L -s covid19.trackercli.com/il
╔══════════════════════════════════════════════════════════════════════╗
║ COVID-19 Tracker CLI v3.1.0 - Israel Update                          ║
╟──────────────────────────────────────────────────────────────────────╢
║ As of 4/6/2020, 6:55:12 AM [Date:4/6/2020]                           ║
╟─────────────╤──────────────╤───────────╤─────────────╤───────────────╢
║ Cases       │ Deaths       │ Recovered │ Active      │ Cases/Million ║
╟─────────────┼──────────────┼───────────┼─────────────┼───────────────╢
║ 8,611       │ 51           │ 585       │ 7,975       │ 995           ║
╟─────────────┼──────────────┼───────────┼─────────────┼───────────────╢
║ Today Cases │ Today Deaths │ Critical  │ Mortality % │ Recovery %    ║
╟─────────────┼──────────────┼───────────┼─────────────┼───────────────╢
║ 181         │ 2            │ 141       │ 0.59        │ 6.79          ║
╟─────────────╧──────────────╧───────────╧─────────────╧───────────────╢
║ Source: https://www.worldometers.info/coronavirus/                   ║
╟──────────────────────────────────────────────────────────────────────╢
║ Code: https://github.com/warengonzaga/covid19-tracker-cli            ║
╚══════════════════════════════════════════════════════════════════════╝
```

Historical data:  
```Bash
$ curl -L -s covid19.trackercli.com/history/il
```



<a name="ee0"></a>
### Force curl not to show the progress bar   
Use `-s` option to make it silent:  
```Bash
curl -o index.html -s https://yurisk.info
```
<hr>


<a name="ee1"></a>
### Download a web page via GET request setting Chrome version 74 as the User-Agent.
Use `-A` to set User-Agent.  
```Bash
curl -o Index.html  -A  "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36" http://example.com
```

Resources:  <a href="https://developers.whatismybrowser.com/useragents/explore/" target="_blank" rel="noopener">  https://developers.whatismybrowser.com/useragents/explore/</a>

<a name="ee2"></a>
### Download a web page via GET request setting Googlebot version 2.1 as the User-Agent.

```Bash
curl -o Index.html -A "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)"  http://example.com
```

<a name="ee3"></a>
### Download a page via https ignoring ceritficate errors
Add `-k` to ignore any SSL certificate warnings/errors.  
```Bash
curl -k -o Index.html https://example.com
```


<a name="ee4"></a>  
### Download a page using SOCKS5 proxy listening on 127.0.0.1  port 10443

```Bash
curl -x socks5://localhost:10443  https://yurisk.info
```

<a name="ee5"></a>
### Download a page using SOCKS5 proxy listening on 127.0.0.1 port 10443 and use remote host to resolve the hostname

```Bash
curl -x socks5h://localhost:10443  https://yurisk.info
```

The idea here is to tunnel DNS requests to the remote end of the tunnel as well, for example for privacy concerns to prevent <a href="https://en.wikipedia.org/wiki/DNS_leak" target="_blank" rel="noopener">DNS leak</a>.


<a name="ee6"></a>  
### Download a page and report time spent in every step starting with resolving:

Source: <a href="https://stackoverflow.com/questions/18215389/how-do-i-measure-request-and-response-times-at-once-using-curl" target="_blank" rel="noopener"> Stackoverflow</a>

- Step 1: Put the parameters to write into a file called say _curl-params_ (just for the convenience instead of CLI):  

```Bash
    time_namelookup:  %{time_namelookup}\n
       time_connect:  %{time_connect}\n
    time_appconnect:  %{time_appconnect}\n
   time_pretransfer:  %{time_pretransfer}\n
      time_redirect:  %{time_redirect}\n
 time_starttransfer:  %{time_starttransfer}\n
                    ----------\n
         time_total:  %{time_total}\n
```

- Step 2: Run the curl supplying this file _curl-params_:

```Bash
curl -w "@curl-params" -o /dev/null -s https://example.com
```

```Bash
 
    time_namelookup:  0.062
       time_connect:  0.062
    time_appconnect:  0.239
   time_pretransfer:  0.239
      time_redirect:  0.000
 time_starttransfer:  0.240
                    ----------
         time_total:  0.241
```



<a name="ee27"></a>  
### Resolve IP address to the owner's Autonomous System Number by sending POST query with form fields to the Team Cymru whois server
When sending any POST data with form fields, the first task is to get all the fields. The esiest way to do it is to browse to the page, fill the form, open the HTML code and write down fields and their values. I did it for the page at <a href="https://asn.cymru.com/" target=_blank rel="noopener">https://asn.cymru.com/</a>  and noted 5 fields to fill with values, the field to place IP address to query for is `bulk_paste`. In curl you specify field values with `-F 'name=value'`  option:

```bash
curl -s  -X POST -F 'action=do_whois' -F 'family=ipv4' -F 'method_whois=whois' -F 'bulk_paste=35.1.33.192' -F 'submit_paste=Submit' https://asn.cymru.com/cgi-bin/whois.cgi | grep "|"
```

Output:  
```bash
<PRE>AS      | IP               | AS Name
36375   | 35.1.33.192      | UMICH-AS-5, US
```

Resources: <a href="https://ec.haxx.se/http/http-post" target=_blank rel="noopener">https://ec.haxx.se/http/http-post</a>


<a name="ee7"></a>  
### Make sure Curl follows redirections (`Location:`) automatically, using the correct `Referer` on each redirection

```Bash
curl -L -e ';auto' -o index.html https://example.com
```

NOTE: All the downloaded pages will be appended to the same output file, here _index.html_.   

<a name="ee8"></a>  
### Send GET request with digest authentication

```Bash
curl --digest http://user:pass@example.com/login
```

<a name="ee9"></a>
### Download a remote file only if it's newer than the local copy

```Bash
curl -z index.html -o index.html https://example.com/index.html 
```

NOTE: file to compare/download, here _index.html_, is compared for timestamp only, no content hashing or anything else.


<a name="ee10"></a>  
### Enable support for compressed encoding in response, as a real browser would do

```Bash
curl -compressed  -o w3.css https://yurisk.info/theme/css/w3.css
```

Note: this option causes curl to sent `Accept-Encoding: gzip` in the request.


<a name="ee11"></a>
### Verify CORS functionality of a website

```Bash
curl -H "Access-Control-Request-Method: GET" -H "Origin: http://localhost" --head https://yurisk.info/2020/03/05/fortiweb-cookbook-content-routing-based-on-url-in-request-configuration/pic1.png
```

Output:

```Bash
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: GET

```


<a name="ee12"></a>
### Convert Curl command into ready to be compiled C source  file

```Bash
curl -o index.html https://yurisk.info --libcurl index.c
```


The output file index.c will contain the source code to implement the same command using Curl C library:

```C

/********* Sample code generated by the curl command line tool **********
 * All curl_easy_setopt() options are documented at:
 * https://curl.haxx.se/libcurl/c/curl_easy_setopt.html
 ************************************************************************/
#include <curl/curl.h>

int main(int argc, char *argv[])
{
  CURLcode ret;
  CURL *hnd;

  hnd = curl_easy_init();
  curl_easy_setopt(hnd, CURLOPT_BUFFERSIZE, 102400L);
  curl_easy_setopt(hnd, CURLOPT_URL, "https://yurisk.info");
  curl_easy_setopt(hnd, CURLOPT_USERAGENT, "curl/7.66.0");
  curl_easy_setopt(hnd, CURLOPT_MAXREDIRS, 50L);
  curl_easy_setopt(hnd, CURLOPT_HTTP_VERSION, (long)CURL_HTTP_VERSION_2TLS);
  curl_easy_setopt(hnd, CURLOPT_SSH_KNOWNHOSTS, "/home/yuri/.ssh/known_hosts");
  curl_easy_setopt(hnd, CURLOPT_TCP_KEEPALIVE, 1L);

  /* Here is a list of options the curl code used that cannot get generated
     as source easily. You may select to either not use them or implement
     them yourself.

  CURLOPT_WRITEDATA set to a objectpointer
  CURLOPT_INTERLEAVEDATA set to a objectpointer
  CURLOPT_WRITEFUNCTION set to a functionpointer
  CURLOPT_READDATA set to a objectpointer
  CURLOPT_READFUNCTION set to a functionpointer
  CURLOPT_SEEKDATA set to a objectpointer
  CURLOPT_SEEKFUNCTION set to a functionpointer
  CURLOPT_ERRORBUFFER set to a objectpointer
  CURLOPT_STDERR set to a objectpointer
  CURLOPT_HEADERFUNCTION set to a functionpointer
  CURLOPT_HEADERDATA set to a objectpointer

  */

  ret = curl_easy_perform(hnd);

  curl_easy_cleanup(hnd);
  hnd = NULL;

  return (int)ret;
}
/**** End of sample code ****/

```

You can now compile it to executable, provided you have `libcurl` library and its headers installed: `gcc index.c -lcurl -o index`


<a  name="ee13"></a>  
### Display just the HTTP response code 

```Bash
curl -w  '%{http_code}' --silent -o /dev/null https://yurisk.info
```

Output:  
```Bash
200
```


<a name="ee25"></a>  
### Get a page using specific version of HTTP protocol
```bash
 curl --http2  -s  -O  https://yurisk.info
```




<a name="ee14"></a>  
### Download file with SCP protocol 

```Bash
 curl scp://99.23.5.18:/root/pdf.pdf -o pdf.pdf -u root
```

Note: curl checks `~/.ssh/known_hosts`  file to verify authenticityy of the remote server. If the remote server is not already in the `known_hosts`, curl will refuse to connect. To prevent it - connect to the remote server via SSH, this will add it to the known hosts. Also, curl should be compiled with support for `libssh2` library.

<a name="ee15"></a>  
### Get external IP address of the machine where the curl is installed 

```Bash
 curl -s http://whatismyip.akamai.com/
87.123.255.103
```


<a name="ee16"></a>  
### Send e-mail via SMTP
First, put the message body and From/To/Subject fields in a file:  
```Bash
# cat message.txt
From: Joe Dow <joedow@example.com>
To: Yuri <yuri@yurisk.info>
Subject: Testing curl SMTP sending

Hi, curl can now send e-mails as well!
```

Now, send the e-mail using the created file and setting e-mail envelope on the CLI:
```Bash
curl -v  smtp://aspmx.l.google.com/smtp.example.com  --mail-from Joedow@example.com  --mail-rcpt yuri@yurisk.info  --upload-file message.txt
```

Here:  
`aspmx.l.google.com`  - the mail server for the recipient domain (`curl` does NOT look for the MX record itself).  
`smtp.example.com` (Optional) - domain the `curl` will use in greeting the mail server (HELO/EHLO).  
`--mail-from` - sender address set in the envelope.  
`--mail-rcpt` - recipient for the mail set in the envelope.

NOTE:  the mail sending is subject to all the anti-spam checks by the receiving mail server, so I recommend to run this with the `-v` option set to see what is going on in real-time.


<a name="ee17"></a>
### Make curl resolve a hostname to the custom IP address you specify without modifying hosts file or using DNS server hacks
Useful to test local copy of a website.  
Problem: You want curl to reach a website "example.com" at IP address 127.0.0.1 without changing local `hosts` file or setting up fake DNS server.  
Solution: Use `--resolve` to specify IP address for a hostname, so curl uses it without querying real DNS servers.

```Bash
curl -v  --resolve "example.com:80:127.0.0.1" http://example.com
```

```Bash
* Added example.com:80:127.0.0.1 to DNS cache
* Hostname example.com was found in DNS cache
*   Trying 127.0.0.1:80...
* Connected to example.com (127.0.0.1) port 80 (#0)
> GET / HTTP/1.1
> Host: example.com
> User-Agent: curl/7.67.0
> Accept: */*
```


<a name="ee18"></a>
### Show how many redirects were followed fetching the URL
Use `num_redirects` variable for that:  
```Bash
 curl -w '%{num_redirects}' -L  -o /dev/null https://cnn.com -s
2
```


<a name="ee20"></a>  
### Use your browser to prepare the complete curl command via "copy as curl" feature
We can use a regular browser to prepare the complete curl command by just browsing to the target site. For that:  
1. Open Developer Tools - **F12** (works in Chrome and Firefox)  
2. Browse to the target site/page.  
3. In the "Network" tab of the Developer Tools find the item you want to GET with curl, right click on it, find menu "Copy as cURL", click on it - this copies to the clipboard ready-to-run curl command to that asset.


<a name="ee21"></a>  
### Test if a website supports the given cipher suite, e.g. obsolete sslv3 & DES
Helps to monitor servers for obsolete or not yet widely supported cipher suites.
Check if site supports sslv3 (old and dangerously broken):  
```Bash
curl -k  https://yurisk.info:443 -v  --sslv3
```

Output:  
```Bash
curl: (35) error:1408F10B:SSL routines:ssl3_get_record:wrong version number
```

Check if the newest (experimental as of 2020) TLS v1.3 is enabled:  
```Bash
curl -k  https://yurisk.info:443 -v  --tlsv1.3

```

Output:  
```Bash
* OpenSSL SSL_connect: SSL_ERROR_ZERO_RETURN in connection to yurisk.info:443
* Marked for [closure]: Failed HTTPS connection

```

Check if a site supports easily breakable DES algorithm:  
```Bash
curl -k -o /dev/null  https://yurisk.info:443  --ciphers DES  
```

Output:  
```Bash
curl: (59) failed setting cipher list: DES
```


<a name="ee22"></a>
### Fetch multiple pages with predictable pattern in their URLs
If a website has a repeating pattern in naming its resources, we can use **URL globbing**.  curl understands ranges `[start-end]` and lists `{item1,item2,...}`. Ranges can be alphanumeric and are inclusive, i.e. [0-100] starts at 0 and includes up to 100. Ranges optionally accept step/increment value: `[10-100:2]`, here 2 is added on each step.  We can use both, ranges and lists, in the same URL.   
_Output files_: curl remembers the matched glob patterns and we can use them with `-o` to specify custom output filenames.

1. Fetch all pages in https://yurisk.info/category/checkpoint-ngngx<i>NNN</i>.html  where _NNN_ goes from 2 to 9. Pay attention to the single quotes - when using on the Bash command line, the range `[]` and list `{}` operators would be otherwise interpreted by the Bash itself instead of curl.

```Bash
curl -s -O 'https://yurisk.info/category/checkpoint-ngngx[2-9].html'

```

Output directory:  
```Bash
ls
checkpoint-ngngx2.html
checkpoint-ngngx3.html
checkpoint-ngngx4.html
checkpoint-ngngx5.html
checkpoint-ngngx6.html
checkpoint-ngngx7.html
checkpoint-ngngx8.html
checkpoint-ngngx9.html
```

2. Fetch all pages _cisco.html,fortinet.html,linux.html,checkpoint-ngngx.html_ inside the _category_ folder:  

```bash
 curl   -O 'https://yurisk.info/category/{cisco,fortinet,linux,checkpoint-ngngx}.html'
```

Output:   
```Bash
ls -1 *.html
checkpoint-ngngx.html
cisco.html
fortinet.html
linux.html
```

3. Download pages with alphabetical ranges.

```Bash
curl-O -s https://yurisk.info/test[a-z]
```


<a name="ee23"></a>  
### How to prevent errors on URLs that contain brackets
 If the curl uses brackets (square and curly) for ranges (<a name="ee22">see above</a>), how do we work with URLs containing such symbols? By using the `-g` option to curl which turns off globbing. It also means we can't use ranges with URLs that contain brackets.

```bash
curl -g https://example.com/{ids}?site=example.gov
```



<a name="ee24"></a>  
### Github: list names of all public repositories for a given user
To query the user's repositories, the URL should have the form of `https://api.github.com/users/<username>/repos `. For example, let's get all the repositories for `curl` project:

```bash
 curl -s  https://api.github.com/users/curl/repos | awk '/\wname/'
```

Output:  
```bash
    "full_name": "curl/build-images",
    "full_name": "curl/curl",
    "full_name": "curl/curl-cheat-sheet",
    "full_name": "curl/curl-docker",
    "full_name": "curl/curl-for-win",
    "full_name": "curl/curl-fuzzer",
    "full_name": "curl/curl-up",
    "full_name": "curl/curl-www",
    "full_name": "curl/doh",
    "full_name": "curl/fcurl",
    "full_name": "curl/h2c",
    "full_name": "curl/stats",
```
 
_Note:_ Github imposes rate limits on the unauthorized requests, currently 60 requests/hour is the maximum. You can  check how many queries are left with the _X-Ratelimit-Remaining_ header:

```bash
 curl -s -i  https://api.github.com/users/curl/repos | grep X-Ratelimit-Remaining
X-Ratelimit-Remaining: 54`
```



<a name="ee26"></a>  
### Display weather report for a given city
There are many websites to query for weather information on the CLI, most popular seems to be wttr.in, so let's use it to get the weather in Milan:  

```bash
 curl wttr.in/Milan
```

Output:

```bash
Weather report: Milan

    \  /       Partly cloudy
  _ /"".-.     17 °C
    \_(   ).   ↓ 6 km/h
    /(___(__)  10 km
               0.0 mm
                                                       ┌─────────────┐
┌──────────────────────────────┬───────────────────────┤  Mon 04 May ├───────────────────────┬──────────────────────────────┐
│            Morning           │             Noon      └──────┬──────┘     Evening           │             Night            │
├──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┤
│      .-.      Light rain     │      .-.      Light rain     │               Overcast       │               Cloudy         │
│     (   ).    17 °C          │     (   ).    18 °C          │      .--.     17 °C          │      .--.     12 °C          │
│    (___(__)   ↖ 26-36 km/h   │    (___(__)   ↖ 20-28 km/h   │   .-(    ).   ↗ 15-24 km/h   │   .-(    ).   ↗ 13-21 km/h   │
│     ‘ ‘ ‘ ‘   9 km           │     ‘ ‘ ‘ ‘   9 km           │  (___.__)__)  10 km          │  (___.__)__)  10 km          │
│    ‘ ‘ ‘ ‘    1.4 mm | 66%   │    ‘ ‘ ‘ ‘    1.9 mm | 65%   │               0.0 mm | 0%    │               0.0 mm | 0%    │
└──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┘
                                                       ┌─────────────┐
┌──────────────────────────────┬───────────────────────┤  Tue 05 May ├───────────────────────┬──────────────────────────────┐
│            Morning           │             Noon      └──────┬──────┘     Evening           │             Night            │
├──────────────────────────────┼──────────────────────────────┼──────────────────────────────┼──────────────────────────────┤
│    \  /       Partly cloudy  │    \  /       Partly cloudy  │    \  /       Partly cloudy  │               Overcast       │
│  _ /"".-.     19 °C          │  _ /"".-.     20 °C          │  _ /"".-.     20 °C          │      .--.     19 °C          │
│    \_(   ).   ↘ 9-14 km/h    │    \_(   ).   ↙ 9-13 km/h    │    \_(   ).   ↙ 14-21 km/h   │   .-(    ).   ↙ 23-34 km/h   │
│    /(___(__)  10 km          │    /(___(__)  10 km          │    /(___(__)  10 km          │  (___.__)__)  10 km          │
│               0.0 mm | 0%    │               0.0 mm | 0%    │               0.0 mm | 0%    │               0.0 mm | 0%    │
└──────────────────────────────┴──────────────────────────────┴──────────────────────────────┴──────────────────────────────┘

```


### References
- Original post: https://yurisk.info/2020/03/13/curl-cookbook/
 
