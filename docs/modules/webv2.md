# Webv2

The Webv2 module attempts to connect to an HTTP server and extract HTTP headers, redirects, page title, favicon, HTML source code, the web technologies being used and take a screenshot of the web page. It combines and upgrades upon the functionality of _http_, _https_ and _web_.

## Webv2 Request Example

```
curl -v -L https://api.binaryedge.io/v1/tasks -d '{"type":"scan", "options":[{"targets":["X.X.X.X"], "ports":[{"port":80, "protocol":"tcp", "modules":["webv2"], "config":{}}]}]}' -H "X-Token:<Token>"
```

### Webv2 Request Options

These are optional parameters that can alter the behaviour of the module. These options can be inserted into the "config" object on the request.

* https - Whether to use HTTPS instead of HTTP
    * "config":{"https":true}
* http_path - Set HTTP path
    * "config":{"http_path":"/robots.txt"}
* http_method - Set HTTP method
    * "config":{"http_method":"PROPFIND"}
* user_agent - Change HTTP User Agent
    * "config":{"user_agent":"Mozilla /5.0 (Compatible MSIE 9.0;Windows NT 6.1;WOW64; Trident/5.0)"}
* host_header - Change HTTP Host header
    * "config":{"host_header":"www.w3.org"}
* custom_http_headers - Set custom HTTP header
    * "config":{"custom_http_headers": "Accept: */*\\r\\nContent-Length: 186"}
* follow_meta - Whether or not to follow meta refresh tags
    * "config":{"follow_meta":true}
* body_inline - Display body on the JSON event instead of a link for a file with the content
    * "config":{"body_inline":true}
* webapps - Scan the target for known web technologies
    * "config":{"webapps":true}
* render - Render the website, take a screenshot and extract the rendered body content
    * "config":{"render":true}

## Schema

### Webv2 Event Schema

```json
{
  ...
  "result": {
    "data":{
        "request":{
            "url":"string",
            "path":"string",
            "method":"string",
            "headers":"object"
        },
        "response":{
            "title":"string",
            "url":"string",
            "path":"string",
            "protocol_version":"int",
            "redirects":"list",
            "status":{
                "code":"int",
                "message":"string"
            },
            "headers":{
                "headers":"object",
                "header_order":"string",
                "header_order_md5_hash":"string"
            },
            "favicon":{
                "link":"string",
                "mmh3_hash":"string",
                "md5_hash":"string",
                "content":"string"
            },
            "body":{
                "sha256_hash":"string",
                "ssdeep_hash":"string",
                "link":"string",
                "content":"string"
            },
            "rendered":{
                "screenshot":"string",
                "sha256_hash":"string",
                "ssdeep_hash":"string",
                "link":"string",
                "content":"string"
            }
        },
        "apps":[
            {
                "name":"string",
                "confidence":"string",
                "version":"string",
                "cpe":"string",
                "categories":"list"
            }
        ]
    }
}
```

### Contents of the fields:

* request - Request made by the module
  	* url - Requested URL
    * path - Requested path (present in URL)
    * method - Request HTTP method
  	* headers - HTTP headers used in the request
* response - Response from server
    * title - Title of the web page
    * url - Final response URL
    * path - Final response path (present in URL)
    * protocol_version - HTTP version
    * redirects - List of redirects before reaching final response
    * status - Status information about the final response
        * code - Status code
        * message - Status message
    * headers - Header information from the web server
        * headers - HTTP headers received from the server
        * header_order - Fingerprint used to identify the server based on the headers
        * header_order_md5_hash - MD5 hash of the fingerprint
    * favicon - Favicon information from the web page
        * link - URL of the favicon
        * mmh3_hash - MMH3 hash of the favicon content (for compliance with other platforms)
        * md5_hash - MD5 hash of the favicon content
        * content - Full base64 content of the favicon
    * body - HTML body response
        * link - URL of the extracted HTML content (if not inline)
        * content - Full HTML content (if not uploaded)
        * sha256_hash - SHA256 hash of the HTML content
        * ssdeep_hash - SSDeep hash of the HTML content (allows for similarity comparison)
    * rendered - Rendered content of the webpage (optional, only present if _render_ option is used)
        * link - URL of the extracted HTML content (if not inline)
        * content - Full HTML content (if not uploaded)
        * sha256_hash - SHA256 hash of the HTML content
        * ssdeep_hash - SSDeep hash of the HTML content (allows for similarity comparison)
        * screenshot - URL of the screenshot of the rendered webpage
    * apps - Web technologies identified on the server (optional, only present if _webapps_ options is used)
        * name - Name of the technology
        * confidence - Confidence level for the match
        * version - Version of the technology
        * cpe - CPE of the technology
        * categories - Categories of the technology

## Webv2 Event Example

```json
{
...
  "result": {
    "data":{
        "request":{
            "url":"https://badssl.com:443/",
            "path":"/",
            "method":"GET",
            "headers":{
                "accept-encoding":"gzip, deflate",
                "user-agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.79 Safari/537.36"
            }
        },
        "response":{
            "title":"badssl.com",
            "url":"https://badssl.com:443/",
            "path":"/",
            "protocol_version":11,
            "redirects":[

            ],
            "status":{
                "code":200,
                "message":"OK"
            },
            "headers":{
                "headers":{
                    "server":"nginx/1.10.3 (Ubuntu)",
                    "date":"Wed, 12 Feb 2020 00:44:38 GMT",
                    "content-type":"text/html",
                    "last-modified":"Wed, 22 Jan 2020 16:30:37 GMT",
                    "transfer-encoding":"chunked",
                    "connection":"keep-alive",
                    "etag":"W/\"5e2878ad-2e05\"",
                    "cache-control":"no-store",
                    "content-encoding":"gzip"
                },
                "header_order":"server,date,content_type,last_modified,transfer_encoding,connection,etag,cache_control,content_encoding",
                "header_order_md5_hash":"88fd27a2cc374832f4963cf936619d6a"
            },
            "favicon":{
                "link":"https://badssl.com:443/icons/icon-blue.png",
                "mmh3_hash":"1853485295",
                "md5_hash":"669e28337772e81efa05c2c6b00d6e95"
            },
            "body":{
                "sha256_hash":"4a3a21c94f926815893b1cdcb1edf15ccbb8b6cf00bd8ac45ec529f26e618508",
                "ssdeep_hash":"3139323a305449586c627961624b5a637546494d414f726e7545664a78516f674963416c2b555a466e4f4e72495246642f646a6d7a636542653a30697943754a5a7a454434334272",
                "link":"https://s3-eu-west-1.amazonaws.com/be-webshots-clients/c36ce70af0d5c0af68cc571d063870f007e21da94bbd10386e80aa56b365231d3b77.html",
                "content":"<!doctype html>\n<html>\n<head>\n  <meta charset=\"utf-8\" />\n  <title>badssl.com</title>\n  <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n  <link rel=\"shortcut icon\" href=\"/icons/favicon-blue.ico\"/>\n  <link rel=\"apple-touch-icon\" href=\"/icons/icon-blue.png\"/>\n  <link rel=\"stylesheet\" href=\"index.css\">\n  <link rel=\"stylesheet\" href=\"github-ribbon.css\">\n  <script src=\"index.js\"></script>\n\n  <!-- fUnKy -->\n  <link rel=\"stylesheet\" href=\"funky/funky.css\">\n  <script src=\"funky/funky.js\"></script>\n</head>\n<body>\n\n<div class=\"title-bar\" title=\"badssl.com - a memorable site for HTTPS misconfiguration\">\n  badssl.com\n</div>\n\n<div id=\"links\">\n\n<div class=\"column\">\n  <div class=\"group\">\n    <h2 id=\"dashboard\"><span class=\"emoji\">\ud83c\udf9b</span>Dashboard</h2>\n    <a href=\"/dashboard/\" target=\"_blank\" class=\"bullet-list\"><span class=\"icon\"></span>Dashboard</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"certificate\"><span class=\"emoji\">\ud83c\udfab</span>Certificate</h2>\n    <a href=\"https://expired.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>expired</a>\n    <a href=\"https://wrong.host.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>wrong.host</a>\n    <a href=\"https://self-signed.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>self-signed</a>\n    <a href=\"https://untrusted-root.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>untrusted-root</a>\n    <a href=\"https://revoked.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>revoked</a>\n    <a href=\"https://pinning-test.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>pinning-test</a>\n    <hr>\n    <a href=\"https://no-common-name.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>no-common-name</a>\n    <a href=\"https://no-subject.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>no-subject</a>\n    <a href=\"https://incomplete-chain.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>incomplete-chain</a>\n    <hr>\n    <a href=\"https://sha1-intermediate.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>sha1-intermediate</a>\n    <a href=\"https://sha256.badssl.com/\" class=\"good\"><span class=\"icon\"></span>sha256</a>\n    <a href=\"https://sha384.badssl.com/\" class=\"good\"><span class=\"icon\"></span>sha384</a>\n    <a href=\"https://sha512.badssl.com/\" class=\"good\"><span class=\"icon\"></span>sha512</a>\n    <hr>\n    <a href=\"https://1000-sans.badssl.com/\" class=\"good\"><span class=\"icon\"></span>1000-sans</a>\n    <a href=\"https://10000-sans.badssl.com/\" class=\"good\"><span class=\"icon\"></span>10000-sans</a>\n    <hr>\n    <a href=\"https://ecc256.badssl.com/\" class=\"good\"><span class=\"icon\"></span>ecc256</a>\n    <a href=\"https://ecc384.badssl.com/\" class=\"good\"><span class=\"icon\"></span>ecc384</a>\n    <hr>\n    <a href=\"https://rsa2048.badssl.com/\" class=\"good\"><span class=\"icon\"></span>rsa2048</a>\n    <a href=\"https://rsa4096.badssl.com/\" class=\"good\"><span class=\"icon\"></span>rsa4096</a>\n    <a href=\"https://rsa8192.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>rsa8192</a> \n    <hr>\n    <a href=\"https://extended-validation.badssl.com/\" class=\"good\"><span class=\"icon\"></span>extended-validation</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"client-certificate\"><span class=\"emoji\">\ud83c\udf9f</span>Client Certificate</h2>\n    <a href=\"/download/\" target=\"_blank\" class=\"bullet-list\"><span class=\"icon\"></span>Certificate Downloads</a>\n    <a href=\"https://client.badssl.com/\" class=\"good\"><span class=\"icon\"></span>client</a>\n    <a href=\"https://client-cert-missing.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>client-cert-missing</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"mixed-content\"><span class=\"emoji\">\ud83d\uddbc</span>Mixed Content</h2>\n    <a href=\"https://mixed-script.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>mixed-script</a>\n    <a href=\"https://very.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>very</a>\n    <hr>\n    <a href=\"https://mixed.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>mixed</a>\n    <a href=\"https://mixed-favicon.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>mixed-favicon</a>\n    <a href=\"https://mixed-form.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>mixed-form</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"http\"><span class=\"emoji\">\u270f\ufe0f</span>HTTP</h2>\n    <a href=\"http://http.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http</a>\n    <a href=\"http://http-textarea.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-textarea</a>\n    <a href=\"http://http-password.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-password</a>\n    <a href=\"http://http-login.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-login</a>\n    <a href=\"http://http-dynamic-login.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-dynamic-login</a>\n    <a href=\"http://http-credit-card.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-credit-card</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"cipher-suite\"><span class=\"emoji\">\ud83d\udd00</span>Cipher Suite</h2>\n    <a href=\"https://cbc.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>cbc</a>\n    <a href=\"https://rc4-md5.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>rc4-md5</a>\n    <a href=\"https://rc4.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>rc4</a>\n    <a href=\"https://3des.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>3des</a>\n    <a href=\"https://null.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>null</a>\n    <hr>\n    <a href=\"https://mozilla-old.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>mozilla-old</a>\n    <a href=\"https://mozilla-intermediate.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>mozilla-intermediate</a>\n    <a href=\"https://mozilla-modern.badssl.com/\" class=\"good\"><span class=\"icon\"></span>mozilla-modern</a>\n  </div>\n</div><!-- class=\"column\" -->\n\n<div class=\"column\">\n  <div class=\"group\">\n    <h2 id=\"key-exchange\"><span class=\"emoji\">\ud83d\udd11</span>Key Exchange</h2>\n    <a href=\"https://dh480.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh480</a>\n    <a href=\"https://dh512.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh512</a>\n    <a href=\"https://dh1024.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh1024</a>\n    <a href=\"https://dh2048.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>dh2048</a>\n    <hr>\n    <a href=\"https://dh-small-subgroup.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh-small-subgroup</a>\n    <a href=\"https://dh-composite.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh-composite</a>\n    <hr>\n    <a href=\"https://static-rsa.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>static-rsa</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"protocol\"><span class=\"emoji\">\u2194\ufe0f</span>Protocol</h2>\n    <a href=\"https://tls-v1-0.badssl.com:1010/\" class=\"dubious\"><span class=\"icon\"></span>tls-v1-0</a>\n    <a href=\"https://tls-v1-1.badssl.com:1011/\" class=\"dubious\"><span class=\"icon\"></span>tls-v1-1</a>\n    <a href=\"https://tls-v1-2.badssl.com:1012/\" class=\"good\"><span class=\"icon\"></span>tls-v1-2</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"certificate-transparency\"><span class=\"emoji\">\ud83d\udd0d</span>Certificate Transparency</h2>\n    <a href=\"https://invalid-expected-sct.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>invalid-expected-sct</a>\n    <a href=\"https://no-sct.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>no-sct</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"upgrade\"><span class=\"emoji\">\u2b06\ufe0f</span>Upgrade</h2>\n    <a href=\"https://hsts.badssl.com/\" class=\"good\"><span class=\"icon\"></span>hsts</a>\n    <a href=\"https://upgrade.badssl.com/\" class=\"good\"><span class=\"icon\"></span>upgrade</a>\n    <hr>\n    <a href=\"https://preloaded-hsts.badssl.com/\" class=\"good\"><span class=\"icon\"></span>preloaded-hsts</a>\n    <a href=\"https://subdomain.preloaded-hsts.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>subdomain.preloaded-hsts</a>\n    <hr>\n    <a href=\"https://https-everywhere.badssl.com/\" class=\"good\"><span class=\"icon\"></span>https-everywhere</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"ui\"><span class=\"emoji\">\ud83d\udc40</span>UI</h2>\n    <a href=\"https://spoofed-favicon.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>spoofed-favicon</a>\n    <a href=\"https://lock-title.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>lock-title</a>\n    <hr>\n    <a href=\"https://long-extended-subdomain-name-containing-many-letters-and-dashes.badssl.com/\" class=\"good\"><span class=\"icon\"></span>long-extended-subdomain-name-containing-many-letters-and-dashes</a>\n    <a href=\"https://longextendedsubdomainnamewithoutdashesinordertotestwordwrapping.badssl.com/\" class=\"good\"><span class=\"icon\"></span>longextendedsubdomainnamewithoutdashesinordertotestwordwrapping</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"known-bad\"><span class=\"emoji\">\u274c</span>Known Bad</h2>\n    <a href=\"https://superfish.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>(Lenovo) Superfish</a>\n    <a href=\"https://edellroot.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>(Dell) eDellRoot</a>\n    <a href=\"https://dsdtestprovider.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>(Dell) DSD Test Provider</a>\n    <a href=\"https://preact-cli.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>preact-cli</a>\n    <a href=\"https://webpack-dev-server.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>webpack-dev-server</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"chrome\"><span class=\"emoji\"><img class=\"chrome-icon\" src=\"front-page-icons/chrome.svg\"></span>Chrome Tests</h2>\n    <a href=\"https://captive-portal.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>captive-portal</a>\n    <a href=\"https://mitm-software.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>mitm-software</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"defunct\"><span class=\"emoji\">\u2620\ufe0f</span>Defunct</h2>\n    <a href=\"https://sha1-2016.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>sha1-2016</a>\n    <a href=\"https://sha1-2017.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>sha1-2017</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"test-suites\"><span class=\"emoji\">\ud83d\udee0</span>Test Suites</h2>\n    <a href=\"https://testsafebrowsing.appspot.com/\" target=\"_blank\" class=\"external\"><span class=\"icon\"></span>Safe Browsing Tests</a>\n    <a href=\"https://www.ssllabs.com/ssltest/viewMyClient.html\" target=\"_blank\" class=\"external\"><span class=\"icon\"></span>SSL Labs Client Test</a>\n    <a href=\"https://mitm.watch/\" target=\"_blank\" class=\"external\"><span class=\"icon\"></span>mitm.watch</a>\n  </div>\n  <div id=\"preload\" style=\"width: 0; height: 0;\">\n    <!-- <link rel=preload> results in warnings in Chrome: https://crbug.com/661055 -->\n    <!-- Workaround: Load the images in bogus elements. -->\n    <script>\n      window.addEventListener(\"load\", function() {\n        var parent = document.querySelector(\"#preload\");\n        var names = [\"bad-white\",\"dubious-white\",\"good-white\",\"page-white\",\"bullet-list-white\",\"external-white\"]\n        for (var i = 0; i < names.length; i++) {\n          var elem = document.createElement(\"span\");\n          elem.style.backgroundImage = \"url(front-page-icons/\" + names[i] + \".svg)\";\n          parent.appendChild(elem);\n        }\n      });\n    </script>\n  </div>\n</div><!-- class=\"column\" -->\n\n</div><!-- id=\"links\" -->\n\n<h2 class=\"your-browser\">Your Browser:\n  <div id=\"browser-info\">\n    <span class=\"highlight\">\n      <span id=\"ua\"></span><br>\n      <span id=\"os\"></span><br>\n    </span>\n    <span id=\"click-to-copy\">\ud83d\udccb Click to copy</span>\n  </div>\n</h2>\n\n<!-- Start of GitHub ribbon. -->\n<div class=\"github-fork-ribbon-wrapper right-top-bottom\">\n    <div class=\"github-fork-ribbon\">\n        <a href=\"https://github.com/chromium/badssl.com\"><span class=\"icon\"></span>On GitHub</a>\n    </div>\n</div>\n<!-- End of GitHub ribbon. -->\n\n</body>\n</html>\n"
            },
            "rendered":{
                "screenshot":"https://s3-eu-west-1.amazonaws.com/be-webshots-clients/c36ce70af0d5c0af68cc571d063870f007e21da94bbd10386e80aa56b365231d3b77.png",
                "sha256_hash":"d909dbec62ab07c57d721bb59520f3f0275ce348caf6cd9c039d076ee728b7d8",
                "ssdeep_hash":"3139323a376a49586c627961624b5a637546494d414f726e7545664a78516f674963416c2b555a46444f4e72495246642f646a6d6c314a42473a37537943754a5a7a4544346a424c",
                "link":"https://s3-eu-west-1.amazonaws.com/be-webshots-clients/c36ce70af0d5c0af68cc571d063870f007e21da94bbd10386e80aa56b365231d3b77c6f2401f1882dcd997.html",
                "content":"<html><head>\n  <meta charset=\"utf-8\">\n  <title>badssl.com</title>\n  <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n  <link rel=\"shortcut icon\" href=\"/icons/favicon-blue.ico\">\n  <link rel=\"apple-touch-icon\" href=\"/icons/icon-blue.png\">\n  <link rel=\"stylesheet\" href=\"index.css\">\n  <link rel=\"stylesheet\" href=\"github-ribbon.css\">\n  <script src=\"index.js\"></script>\n\n  <!-- fUnKy -->\n  <link rel=\"stylesheet\" href=\"funky/funky.css\">\n  <script src=\"funky/funky.js\"></script>\n</head>\n<body>\n\n<div class=\"title-bar\" title=\"badssl.com - a memorable site for HTTPS misconfiguration\">\n  badssl.com\n</div>\n\n<div id=\"links\">\n\n<div class=\"column\">\n  <div class=\"group\">\n    <h2 id=\"dashboard\"><span class=\"emoji\">\ud83c\udf9b</span>Dashboard</h2>\n    <a href=\"/dashboard/\" target=\"_blank\" class=\"bullet-list\"><span class=\"icon\"></span>Dashboard</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"certificate\"><span class=\"emoji\">\ud83c\udfab</span>Certificate</h2>\n    <a href=\"https://expired.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>expired</a>\n    <a href=\"https://wrong.host.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>wrong.host</a>\n    <a href=\"https://self-signed.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>self-signed</a>\n    <a href=\"https://untrusted-root.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>untrusted-root</a>\n    <a href=\"https://revoked.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>revoked</a>\n    <a href=\"https://pinning-test.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>pinning-test</a>\n    <hr>\n    <a href=\"https://no-common-name.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>no-common-name</a>\n    <a href=\"https://no-subject.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>no-subject</a>\n    <a href=\"https://incomplete-chain.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>incomplete-chain</a>\n    <hr>\n    <a href=\"https://sha1-intermediate.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>sha1-intermediate</a>\n    <a href=\"https://sha256.badssl.com/\" class=\"good\"><span class=\"icon\"></span>sha256</a>\n    <a href=\"https://sha384.badssl.com/\" class=\"good\"><span class=\"icon\"></span>sha384</a>\n    <a href=\"https://sha512.badssl.com/\" class=\"good\"><span class=\"icon\"></span>sha512</a>\n    <hr>\n    <a href=\"https://1000-sans.badssl.com/\" class=\"good\"><span class=\"icon\"></span>1000-sans</a>\n    <a href=\"https://10000-sans.badssl.com/\" class=\"good\"><span class=\"icon\"></span>10000-sans</a>\n    <hr>\n    <a href=\"https://ecc256.badssl.com/\" class=\"good\"><span class=\"icon\"></span>ecc256</a>\n    <a href=\"https://ecc384.badssl.com/\" class=\"good\"><span class=\"icon\"></span>ecc384</a>\n    <hr>\n    <a href=\"https://rsa2048.badssl.com/\" class=\"good\"><span class=\"icon\"></span>rsa2048</a>\n    <a href=\"https://rsa4096.badssl.com/\" class=\"good\"><span class=\"icon\"></span>rsa4096</a>\n    <a href=\"https://rsa8192.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>rsa8192</a> \n    <hr>\n    <a href=\"https://extended-validation.badssl.com/\" class=\"good\"><span class=\"icon\"></span>extended-validation</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"client-certificate\"><span class=\"emoji\">\ud83c\udf9f</span>Client Certificate</h2>\n    <a href=\"/download/\" target=\"_blank\" class=\"bullet-list\"><span class=\"icon\"></span>Certificate Downloads</a>\n    <a href=\"https://client.badssl.com/\" class=\"good\"><span class=\"icon\"></span>client</a>\n    <a href=\"https://client-cert-missing.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>client-cert-missing</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"mixed-content\"><span class=\"emoji\">\ud83d\uddbc</span>Mixed Content</h2>\n    <a href=\"https://mixed-script.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>mixed-script</a>\n    <a href=\"https://very.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>very</a>\n    <hr>\n    <a href=\"https://mixed.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>mixed</a>\n    <a href=\"https://mixed-favicon.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>mixed-favicon</a>\n    <a href=\"https://mixed-form.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>mixed-form</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"http\"><span class=\"emoji\">\u270f\ufe0f</span>HTTP</h2>\n    <a href=\"http://http.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http</a>\n    <a href=\"http://http-textarea.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-textarea</a>\n    <a href=\"http://http-password.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-password</a>\n    <a href=\"http://http-login.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-login</a>\n    <a href=\"http://http-dynamic-login.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-dynamic-login</a>\n    <a href=\"http://http-credit-card.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>http-credit-card</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"cipher-suite\"><span class=\"emoji\">\ud83d\udd00</span>Cipher Suite</h2>\n    <a href=\"https://cbc.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>cbc</a>\n    <a href=\"https://rc4-md5.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>rc4-md5</a>\n    <a href=\"https://rc4.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>rc4</a>\n    <a href=\"https://3des.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>3des</a>\n    <a href=\"https://null.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>null</a>\n    <hr>\n    <a href=\"https://mozilla-old.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>mozilla-old</a>\n    <a href=\"https://mozilla-intermediate.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>mozilla-intermediate</a>\n    <a href=\"https://mozilla-modern.badssl.com/\" class=\"good\"><span class=\"icon\"></span>mozilla-modern</a>\n  </div>\n</div><!-- class=\"column\" -->\n\n<div class=\"column\">\n  <div class=\"group\">\n    <h2 id=\"key-exchange\"><span class=\"emoji\">\ud83d\udd11</span>Key Exchange</h2>\n    <a href=\"https://dh480.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh480</a>\n    <a href=\"https://dh512.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh512</a>\n    <a href=\"https://dh1024.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh1024</a>\n    <a href=\"https://dh2048.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>dh2048</a>\n    <hr>\n    <a href=\"https://dh-small-subgroup.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh-small-subgroup</a>\n    <a href=\"https://dh-composite.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>dh-composite</a>\n    <hr>\n    <a href=\"https://static-rsa.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>static-rsa</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"protocol\"><span class=\"emoji\">\u2194\ufe0f</span>Protocol</h2>\n    <a href=\"https://tls-v1-0.badssl.com:1010/\" class=\"dubious\"><span class=\"icon\"></span>tls-v1-0</a>\n    <a href=\"https://tls-v1-1.badssl.com:1011/\" class=\"dubious\"><span class=\"icon\"></span>tls-v1-1</a>\n    <a href=\"https://tls-v1-2.badssl.com:1012/\" class=\"good\"><span class=\"icon\"></span>tls-v1-2</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"certificate-transparency\"><span class=\"emoji\">\ud83d\udd0d</span>Certificate Transparency</h2>\n    <a href=\"https://invalid-expected-sct.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>invalid-expected-sct</a>\n    <a href=\"https://no-sct.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>no-sct</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"upgrade\"><span class=\"emoji\">\u2b06\ufe0f</span>Upgrade</h2>\n    <a href=\"https://hsts.badssl.com/\" class=\"good\"><span class=\"icon\"></span>hsts</a>\n    <a href=\"https://upgrade.badssl.com/\" class=\"good\"><span class=\"icon\"></span>upgrade</a>\n    <hr>\n    <a href=\"https://preloaded-hsts.badssl.com/\" class=\"good\"><span class=\"icon\"></span>preloaded-hsts</a>\n    <a href=\"https://subdomain.preloaded-hsts.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>subdomain.preloaded-hsts</a>\n    <hr>\n    <a href=\"https://https-everywhere.badssl.com/\" class=\"good\"><span class=\"icon\"></span>https-everywhere</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"ui\"><span class=\"emoji\">\ud83d\udc40</span>UI</h2>\n    <a href=\"https://spoofed-favicon.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>spoofed-favicon</a>\n    <a href=\"https://lock-title.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>lock-title</a>\n    <hr>\n    <a href=\"https://long-extended-subdomain-name-containing-many-letters-and-dashes.badssl.com/\" class=\"good\"><span class=\"icon\"></span>long-extended-subdomain-name-containing-many-letters-and-dashes</a>\n    <a href=\"https://longextendedsubdomainnamewithoutdashesinordertotestwordwrapping.badssl.com/\" class=\"good\"><span class=\"icon\"></span>longextendedsubdomainnamewithoutdashesinordertotestwordwrapping</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"known-bad\"><span class=\"emoji\">\u274c</span>Known Bad</h2>\n    <a href=\"https://superfish.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>(Lenovo) Superfish</a>\n    <a href=\"https://edellroot.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>(Dell) eDellRoot</a>\n    <a href=\"https://dsdtestprovider.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>(Dell) DSD Test Provider</a>\n    <a href=\"https://preact-cli.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>preact-cli</a>\n    <a href=\"https://webpack-dev-server.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>webpack-dev-server</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"chrome\"><span class=\"emoji\"><img class=\"chrome-icon\" src=\"front-page-icons/chrome.svg\"></span>Chrome Tests</h2>\n    <a href=\"https://captive-portal.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>captive-portal</a>\n    <a href=\"https://mitm-software.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>mitm-software</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"defunct\" class=\"hover-link\"><span class=\"emoji\">\u2620\ufe0f</span>Defunct</h2>\n    <a href=\"https://sha1-2016.badssl.com/\" class=\"dubious\"><span class=\"icon\"></span>sha1-2016</a>\n    <a href=\"https://sha1-2017.badssl.com/\" class=\"bad\"><span class=\"icon\"></span>sha1-2017</a>\n  </div>\n  <div class=\"group\">\n    <h2 id=\"test-suites\"><span class=\"emoji\">\ud83d\udee0</span>Test Suites</h2>\n    <a href=\"https://testsafebrowsing.appspot.com/\" target=\"_blank\" class=\"external\"><span class=\"icon\"></span>Safe Browsing Tests</a>\n    <a href=\"https://www.ssllabs.com/ssltest/viewMyClient.html\" target=\"_blank\" class=\"external\"><span class=\"icon\"></span>SSL Labs Client Test</a>\n    <a href=\"https://mitm.watch/\" target=\"_blank\" class=\"external\"><span class=\"icon\"></span>mitm.watch</a>\n  </div>\n  <div id=\"preload\" style=\"width: 0; height: 0;\">\n    <!-- <link rel=preload> results in warnings in Chrome: https://crbug.com/661055 -->\n    <!-- Workaround: Load the images in bogus elements. -->\n    <script>\n      window.addEventListener(\"load\", function() {\n        var parent = document.querySelector(\"#preload\");\n        var names = [\"bad-white\",\"dubious-white\",\"good-white\",\"page-white\",\"bullet-list-white\",\"external-white\"]\n        for (var i = 0; i < names.length; i++) {\n          var elem = document.createElement(\"span\");\n          elem.style.backgroundImage = \"url(front-page-icons/\" + names[i] + \".svg)\";\n          parent.appendChild(elem);\n        }\n      });\n    </script>\n  <span style=\"background-image: url(&quot;front-page-icons/bad-white.svg&quot;);\"></span><span style=\"background-image: url(&quot;front-page-icons/dubious-white.svg&quot;);\"></span><span style=\"background-image: url(&quot;front-page-icons/good-white.svg&quot;);\"></span><span style=\"background-image: url(&quot;front-page-icons/page-white.svg&quot;);\"></span><span style=\"background-image: url(&quot;front-page-icons/bullet-list-white.svg&quot;);\"></span><span style=\"background-image: url(&quot;front-page-icons/external-white.svg&quot;);\"></span></div>\n</div><!-- class=\"column\" -->\n\n</div><!-- id=\"links\" -->\n\n<h2 class=\"your-browser\">Your Browser:\n  <div id=\"browser-info\">\n    <span class=\"highlight\">\n      <span id=\"ua\" title=\"Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Ubuntu Chromium/79.0.3945.79 HeadlessChrome/79.0.3945.79 Safari/537.36\">Chrome 79.0.3945.79</span><br>\n      <span id=\"os\">Linux</span><br>\n    </span>\n    <span id=\"click-to-copy\">\ud83d\udccb Click to copy</span>\n  </div>\n</h2>\n\n<!-- Start of GitHub ribbon. -->\n<div class=\"github-fork-ribbon-wrapper right-top-bottom\">\n    <div class=\"github-fork-ribbon\">\n        <a href=\"https://github.com/chromium/badssl.com\"><span class=\"icon\"></span>On GitHub</a>\n    </div>\n</div>\n<!-- End of GitHub ribbon. -->\n\n\n\n</body></html>"
            }
        },
        "apps":[
            {
                "name":"Nginx",
                "confidence":"100",
                "version":"1.10.3",
                "cpe":"cpe:/a:nginx:nginx:1.10.3",
                "categories":[
                    "web servers",
                    "reverse proxy"
                ]
            },
            {
                "name":"Ubuntu",
                "confidence":"100",
                "categories":[
                    "operating systems"
                ]
            }
        ]
    }
  }  
}
```
