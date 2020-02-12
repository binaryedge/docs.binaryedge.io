# web-enrich

The web-enrich module attempts to connect to an HTTP server and extract HTTP headers, redirects, HTML source code, the web technologies and enrich data that the module webv2 couldn't find or doesn't look for.

## web-enrich Request Example

```
curl -v -L https://api.binaryedge.io/v1/tasks -d '{"type":"scan", "options":[{"targets":["X.X.X.X"], "ports":[{"port":80, "protocol":"tcp", "modules":["web-enrich"], "config":{}}]}]}' -H "X-Token:<Token>"
```

### web-enrich Request Options

These are optional parameters that can alter the behaviour of the module. These options can be inserted into the "config" object on the request.

- extended - Extend functionalities of plugins. This could be tests that would take some time
    - "config":{"extended":true}

## Schema

### web-enrich Event Schema

```json
{
    ...
    "result": {
        "data": {
            "http_version":"string",
            "ssl":"bool",
            "fqdn":"string",
            "headers":"object",
            "redirects":"list",
            "url":"string",
            "name":"string",
            "category":"string",
            "drupal": {
                "version":"list",
                "cpe":"string",
                "confidence":"int",
                "modules":"list"
            },
            "f5_bigip": {

            },
            "joomla": {
                "version":"list",
                "cpe":"string",
                "confidence":"int",
                "components":"list",
                "modules":"list",
                "plugins":"list",
                "templates":"list"
            },
            "magento": {
                "version":"list",
                "type":"string",
                "cpe":"string",
                "confidence":"int",
                "cache_leak":"bool",
                "internal_path":"string"
            },
            "umbraco": {
                "version":"list",
                "cpe":"string",
                "confidence":"int",
                "internal_information": {
                    "physical_path":"string",
                    "local_address":"string",
                    "remote_address":"string",
                    "remote_host":"string",
                    "remote_port":"string",
                    "http_x_forwarded_for":"string",
                    "http_x_forwarded_for_port":"string"
                }
            },
            "wordpress": {
                "version":"list",
                "cpe":"string",
                "confidence":"int",
                "plugins":"list",
                "users":"list",
                "directory_listing":"list",
                "internal_path":"string"
            },
            "citrix_netscaler": {
                "name":"string",
                "host":"string",
                "port":"string"
            },
            "f5_bigip_loadbalancer": {
                "pool_name":"string",
                "host":"string",
                "port":"int",
                "route_domain":"string",
                "ipv6":"bool"
            },
            "secrets": {
                "amazon_aws_s3_url":"string",
                "square_oauth_secret":"string"
            }
        }
    }
}
```

### Contents of the fields:

- http_version - HTTP version
- ssl - If the connection was made in HTTPS
- fqdn - Fully Qualified Domain name of the web server
- headers - Header information from the web server
- redirects - List of redirects before reaching final response
- url - Calculated url of the web application
- name - Name of the technology
- category - Categories of the technology
- drupal - Drupal Plugin
    - version - List with the version or possible versions of the technology
    - cpe - CPE of the technology
    - confidence - Confidence level for the match (decreases based on checks for a full match)
    - modules - Information about the modules present
        - TBD
- f5_bigip - F5 BigIP Plugin
- joomla - Joomla Plugin
    - version - List with the version or possible versions of the technology
    - cpe - CPE of the technology
    - confidence - Confidence level for the match (decreases based on checks for a full match)
    - components - Information about the components present
        - TBD
    - modules - Information about the modules present
        - TBD
    - plugins - Information about the plugins present
        - TBD
    - templates - Information about the templates present
        - TBD
- magento - Magento Plugin
    - version - List with the version or possible versions of the technology
    - type - Type of Magento Installation (Community or Enterprise)
    - cpe - CPE of the technology
    - confidence - Confidence level for the match (decreases based on checks for a full match)
    - cache_leak - Check of Cache Leak problem
    - internal_path - Internal path of the web application
- umbraco - Umbraco Plugin
    - version - List with the version or possible versions of the technology
    - cpe - CPE of the technology
    - confidence - Confidence level for the match (decreases based on checks for a full match)
    - internal_information - Internal WebApp/Machine Information
        - physical_path - Internal path of the web application
        - local_address - Local address of the machine
        - remote_address - Remote address of the machine
        - remote_host - Remote Host of the connection
        - remote_port - Remote Port of the connection
        - http_x_forwarded_for - Host of connection through HTTP Proxy or Load balancer
        - http_x_forwarded_for_port - Port of connection through HTTP Proxy or Load balancer
- wordpress - WordPress plugin
    - version - List with the version or possible versions of the technology
    - cpe - CPE of the technology
    - confidence - Confidence level for the match (decreases based on checks for a full match)
    - plugins - Information about the plugins present
        - TBD
    - users - Information about the users present 
        - TBD
    - directory_listing - Paths that have directory listing
    - internal_path - Internal path of the web application
- citrix_netscaler - Citrix Netscaler plugin
    - name - Name of the Citrix Netscaler cookie
    - host - Host of the Citrix Netscaler machine
    - port - Port of the Citrix Netscaler machine
- f5_bigip_loadbalancer - F5 BigIP Load Balancer Plugin
    - pool_name - Pool Name of the F5 BigIP Load Balancer
    - host - Host of the F5 BigIP Load Balancer
    - port - Port of the F5 BigIP Load Balancer
    - route_domain - Route Domain of the F5 BigIP Load Balancer
    - ipv6 - If host is IPv6
- secrets - Secrets Plugin
    - amazon_aws_s3_url - Amazon s3 bucket url
    - square_oauth_secret - Square oauth secret


## web-enrich Event Example

```json
{
    ...
    "result": {
        "data":{
            "url":"https://www.intactinfo.com/",
            "name":"WordPress",
            "category":[
                "CMS",
                "Blogs"
            ],
            "wordpress":{
                "version":[
                    "4.9.13"
                ],
                "cpe":"cpe:/a:wordpress:wordpress:4.9.13",
                "confidence":100,
                "plugins":[
                    {
                        "name":"WP Social Icons",
                        "slug":"wp-social-icons",
                        "version":"1.1",
                        "site_url":"https://www.intactinfo.com/wp-content/plugins/wp-social-icons/readme.txt",
                        "plugin_url":"https://plugins.svn.wordpress.org/wp-social-icons/"
                    },
                    {
                        "name":"Contact Form 7",
                        "slug":"contact-form-7",
                        "version":"4.4.1",
                        "site_url":"https://www.intactinfo.com/wp-content/plugins/contact-form-7/readme.txt",
                        "plugin_url":"https://plugins.svn.wordpress.org/contact-form-7/"
                    }
                ],
                "users":[
                    {
                        "name":"admin",
                        "username":"admin",
                        "email_md5":"438ad82c7584d955876a96f694069211"
                    },
                    {
                        "name":"Amanda Masick",
                        "username":"amanda",
                        "email_md5":"59f73c3fc839a89d2c5248573631febf"
                    },
                    {
                        "name":"Leslee Detillo",
                        "username":"leslee",
                        "email_md5":"13feed229c1e27813af090afbff8b130"
                    },
                    {
                        "name":"Lizzeth Flores",
                        "username":"lizzeth",
                        "email_md5":"3c51ada4e9066897cb481a500a09a7ec"
                    },
                    {
                        "name":"Michelle Tea",
                        "username":"michelle",
                        "email_md5":"3e0ed170566dd3d6011c3bb27408714a"
                    },
                    {
                        "name":"Roberto Bustamante",
                        "username":"roberto",
                        "email_md5":"ddbefa2bdd2e80de62ae760589c14cd8"
                    },
                    {
                        "name":"Ryan Gaeta",
                        "username":"ryan-g",
                        "email_md5":"1b13ba57e6b8dd6bb3295cfff0555420"
                    },
                    {
                        "name":"Simone Beckom",
                        "username":"simone-beckom",
                        "email_md5":"d13a74767cba97d3e7d5d49293215b64"
                    },
                    {
                        "name":"Sodi Hundal",
                        "username":"sodi",
                        "email_md5":"137a9f37891b210636af93e8b62e2290"
                    }
                ],
                "directory_listing":[
                    "/wp-includes/",
                    "/wp-content/uploads/"
                ],
                "internal_path":""
            },
            "http_version":"HTTP/1.1",
            "ssl":true,
            "fqdn":"www.intactinfo.com",
            "headers":{
                "date":"Wed, 12 Feb 2020 01:15:43 GMT",
                "content-type":"text/html; charset=UTF-8",
                "transfer-encoding":"chunked",
                "connection":"keep-alive",
                "set-cookie":"__cfduid=d3f69fd24fe38afa5984d479c144382501581470142; expires=Fri, 13-Mar-20 01:15:42 GMT; path=/; domain=.intactinfo.com; HttpOnly; SameSite=Lax",
                "x-powered-by":"PHP/7.2.27",
                "x-pingback":"https://www.intactinfo.com/xmlrpc.php",
                "link":"<https://www.intactinfo.com/wp-json/>; rel=\"https://api.w.org/\", <https://www.intactinfo.com/>; rel=shortlink",
                "cache-control":"max-age=600",
                "expires":"Wed, 12 Feb 2020 01:25:43 GMT",
                "vary":"Accept-Encoding,User-Agent",
                "strict-transport-security":"max-age=31536000",
                "cf-cache-status":"DYNAMIC",
                "expect-ct":"max-age=604800, report-uri=\"https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct\"",
                "server":"cloudflare",
                "cf-ray":"563ab6897a2c546c-MAD",
                "content-encoding":"gzip"
            },
            "redirects":[
                {
                    "status_code":301,
                    "redirect_uri":"http://68.171.209.56:80/",
                    "headers":{
                        "date":"Wed, 12 Feb 2020 01:15:41 GMT",
                        "server":"Apache",
                        "x-powered-by":"PHP/7.2.27",
                        "x-pingback":"http://www.intactinfo.com/xmlrpc.php",
                        "location":"https://68.171.209.56/",
                        "cache-control":"max-age=600",
                        "expires":"Wed, 12 Feb 2020 01:25:41 GMT",
                        "strict-transport-security":"max-age=31536000",
                        "vary":"User-Agent",
                        "content-length":"0",
                        "keep-alive":"timeout=5, max=100",
                        "connection":"Keep-Alive",
                        "content-type":"text/html; charset=UTF-8"
                    }
                },
                {
                    "status_code":301,
                    "redirect_uri":"https://68.171.209.56/",
                    "headers":{
                        "date":"Wed, 12 Feb 2020 01:15:42 GMT",
                        "server":"Apache",
                        "x-powered-by":"PHP/7.2.27",
                        "x-pingback":"https://www.intactinfo.com/xmlrpc.php",
                        "location":"https://www.intactinfo.com/",
                        "cache-control":"max-age=600",
                        "expires":"Wed, 12 Feb 2020 01:25:42 GMT",
                        "strict-transport-security":"max-age=31536000",
                        "vary":"User-Agent",
                        "content-length":"0",
                        "keep-alive":"timeout=5, max=100",
                        "connection":"Keep-Alive",
                        "content-type":"text/html; charset=UTF-8"
                    }
                }
            ]
        }
    }
}
```