# Host Search Parameters

The API has endpoints for querying our data in which you can use free text search together with one or more of the filters listed below.


## Notes

**Free Text**: not specifying a field will search on the full records, which can include other information not stated below. Although free text search without specifying fields is available, it might be processed differently from searching on specific fields. For better results, always specify search fields.

**Conditionals**: the following conditionals are available: NOT, AND, OR. Must be UPPERCASE. You can also use the minus sign (-) as a replacement for the NOT conditional. By default, a sequence of search terms without conditionals will be interpreted as if using the AND conditional. 

**Comparison**: you can use comparison operators on number fields. E.g. _field:>100_.

**Field existence or omission**: you can search for records that have a specific field by using _\_exists\_:field_. Conversely, for records missing a field it would be _NOT \_exists\_:field_.

**Wildcards**: you can use wildcards on your query terms. However, for the best results and performance, try to be as specific as you can. E.g. _field:security*_

**String fields**: if the string is expected to have spaces, some kind of punctuation in the middle, or special symbols, try quoting the search terms, i.e. instead of querying _field:value_ try _field:"value"_. You can also try instead _field.keyword:"value"_. The first one will search for any occurrence of any of the words in _value_, while the second one will search for an exact match of the string. Finally, in the case of some special symbols, you might require escaping in order to make the query valid.

**Regular Expressions**: regular expressions are not supported at the moment.


## General Fields

### as_name: (string)
Search by AS name. 

    e.g. as_name:amazon

### asn: (int)
Search by ASN. 

    e.g. asn:4812

### country: (string) 
Search using ISO2 Country Codes. 
    
    e.g. country:ES

### created_at: (date)
Search by timestamp.

    e.g.
        created_at:[2018-09-01 TO 2018-10-01]
        created_at:2018-09-01

### ip: (string) 
Search by IP address or CIDR. 

    e.g ip:"192.168.1.1/24" or ip:192.168.1.1

### ipv4: (boolean)
Search for IPv4 results:

    e.g ipv4:true

### ipv6: (boolean)
Search for IPv6 results:

    e.g ipv6:true

### geoip.city_name: (string) 
Search using city names. 
    
    e.g. geoip.city_name:madrid

### geoip.country_name: (string) 
Search using country names. 
    
    e.g. geoip.country_name:spain

### has_screenshot: (boolean) 
Search for screenshots, true or false (VNC, RDP or X11 module types only). 

    e.g. has_screenshot:true

### port: (int) 
Search by port number. 
    
    e.g. port:80

### protocol: (string)
Search by protocol. Can be TCP or UDP. 
    
    e.g. protocol:tcp

### rdns: (string)
Search by RDNS.

    e.g. rdns:a23-52-79-104.deploy.static.akamaitechnologies.com

### rdns_parent: (string)
Search by RDNS root.

    e.g. rdns_parent:akamaitechnologies.com

### type: (string)
Search by event type. Can be service-simple (the default service identification module), ssl, ssh, vnc, rdp, x11, mongodb, memcached, elasticsearch, redis.

    e.g. type:ssl

### tag: (string)
Search by tags. Can be ICS, MALWARE, DATABASE, WEBSERVER, IOT, CAMERA. Tag list and matches are constantly being updated.

    e.g. tag:IOT

#### Available tags

* BUSYBOX
* CAMERA
* DATABASE
* DEVICES
* GAMES
* ICS
* IOT
* SHELL
* WEBCAM
* WEBSERVER


## Service-Simple

* type:service-simple
* Our service identification module.

### banner: (string) 
Search by banner.
    
    e.g. banner:admin

### cpe: (string) 
Search by CPE.
    
    e.g. cpe.keyword:"cpe:/a:lighttpd:lighttpd"

### device: (string)
Search by device type.

    e.g. device:webcam

### extrainfo: (string)
Search by extra info (can include information such as build, extensions, OS, etc).

    e.g. extrainfo:"PHP/5.4.19"

### name: (string) 
Search by service names.
    
    e.g. service:http

### ostype: (string)
Search by OS type.

    e.g. ostype:Windows

### product: (string) 
Search by product names. 
    
    e.g. product:nginx

### version: (string) 
Search by product versions. Better used together with product.
    
    e.g. version:1.1.0


## RDP

### reason: (string)
Search by RDP status reason.

    e.g. rdp.reason:error

### security: (string)
Search by RDP security detected.

    e.g. rdp.security:NLA


## Bluekeep

### vulnerable: (boolean)
Search by whether an RDP server is vulnerable to Bluekeep or not.

    e.g. bluekeep.vulnerable:NLA


## VNC

### auth_enabled: (boolean)
Search by whether VNC has auth enabled or not.

    e.g. vnc.auth_enabled:false

### height: (int)
Search by VNC height.

    e.g. vnc.height:768

### msg: (string)
Search by VNC returned message.

    e.g. vnc.msg:incompatible

### title: (string)
Search by VNC title.

    e.g. vnc.title:android

### version: (string)
Search by VNC version.

    e.g. vnc.version:"3.8"

### width: (int)
Search by VNC width.

    e.g. vnc.width:1024


## X11

### connected: (boolean)
Search by whether X11 server was successfully connected to or not.

    e.g. x11.connected:true

### height: (int)
Search by X11 height.

    e.g. x11.height:768

### vendor: (string)
Search by X11 vendor.

    e.g. x11.vendor:"The X.Org Foundation"

### vendor_release: (string)
Search by X11 vendor release.

    e.g. x11.vendor_release:"10706000"

### version: (string)
Search by X11 version.

    e.g. x11.version:"11.0"

### width: (int)
Search by X11 width.

    e.g. x11.width:1024


## SSH

### compression: (string)
Search by compression algorithms.

    e.g. ssh.algorithms.compression:zlib

### encryption: (string)
Search by encryption algorithms.

    e.g. ssh.algorithms.encryption.keyword:"aes256-cbc"

### kex: (string)
Search by Key Exchange algorithms.

    e.g. ssh.algorithms.kex.keyword:"diffie-hellman-group-exchange-sha256"

### mac (string)
Search by Message Authentication Code algorithms.

    e.g. ssh.algorithms.mac.keyword:"hmac-sha1"

### server_host_key: (string)
Search by Host key encryption.

    e.g. ssh.algorithms.server_host_key.keyword:"ssh-rsa"

### banner: (string)
Search by banner.

    e.g. ssh.banner.keyword:"SSH-2.0-OpenSSH_LeadSec"

### cyphers: (string)
Search by SSH cyphers.

    e.g. ssh.cyphers:"ssh-rsa"

### fingerprint: (string)
Search by SSH fingerprints.

    e.g. ssh.fingerprint:"c0:76:ed:4a:b6:85:7f:cb:b8:ff:20:ac:fc:a9:aa:fb, e9:d6:05:d1:a2:55:76:aa:bb:d8:18:15:ac:b9:01:4b"

### hassh: (string)
Search by HASSH hash.

    e.g. ssh.hassh:0f5053d1cc689128b6db47f340f3285f

### hassh_algorithms: (string)
Search by HASSH algorithms string.

    e.g. ssh.hassh_algorithms:"diffie-hellman-group-exchange-sha256,diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1,aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc,arcfour,rijndael-cbc@lysator.liu.se,hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-ripemd160,hmac-ripemd160@openssh.com,hmac-sha1-96,hmac-md5-96,none,zlib@openssh.com"


## SSL

### cert.issuer.common_name: (string)
Search by leaf certificate issuer's Common Name.

    e.g. ssl.cert.issuer.common_name:microsoft

### cert.issuer.common_name: (string)
Search by leaf certificate issuer's Country Name.

    e.g. ssl.cert.issuer.country_name:cn

### cert.issuer.organization_name: (string)
Search by leaf certificate issuer's Organization Name.

    e.g. ssl.cert.issuer.organization_name:microsoft

### cert.issuer.organizational_unit_name: (string)
Search by leaf certificate issuer's Organizational Unit Name.

    e.g. ssl.cert.issuer.organizational_unit_name:microsoft

### cert.issuer_names: (string)
Search by leaf certificate issuer's names (common_name, organization_name, organizational_unit_name combined).

    e.g. ssl.cert.issuer_names:kubernetes

### cert.not_after: (date)
Search by leaf certificate's expiration date.

    e.g. ssl.cert.not_after:[2018-12-01 TO 2019-01-01]
         ssl.cert.not_after:2019-01-01

### cert.not_before: (date)
Search by leaf certificate's creation date.

    e.g. ssl.cert.not_before:[2018-12-01 TO 2019-01-01]
         ssl.cert.not_before:2019-01-01

### cert.public_key_info.algorithm: (string)
Search by Public Key algorithm.

    e.g. cert.public_key_info.algorithm:ec

### cert.public_key_info.curve: (string)
Search by Public Key curve.

    e.g. cert.public_key_info.curve:secp256r1

### cert.public_key_info.key_size: (string)
Search by Public Key key size.

    e.g. cert.public_key_info.key_size:2048

### cert.public_key_info.sha256_fingerprint: (string)
Search by Public Key SHA256 fingerprint.

    e.g. cert.public_key_info.sha256_fingerprint:"4f:53:aa:f3:c6:6b:28:32:3f:77:cf:d7:1b:96:f8:7b:a0:b6:ee:a3:12:a7:62:b1:0a:c5:a1:2d:7d:29:09:e9"

### cert.serial: (string)
Search by leaf certificate's Serial Number.

    e.g. ssl.cert.serial:160000708D70A2A4CB63ABA1C700000000708D

### cert.signature_algorithm: (string)
Search by leaf certificate's signature algorithm.

    e.g. ssl.cert.signature_algorithm:sha256_rsa

### cert.signature_value: (string)
Search by leaf certificate's signature.

    e.g. ssl.cert.signature_value:"	5d:d1:60:d1:57:1f:3f:ba:ed:c1:36:c9:08:fa:7a:8a:53:78:73:5d:93:c9:cc:11:cc:c8:f5:2c:3e:af:aa:12:73:46:1b:99:35:d7:b6:17:1c:ba:19:c0:f0:d1:eb:92:af:60:a4:b8:2d:18:2c:25:43:59:51:a6:74:26:43:73:d9:dd:58:0b:6f:ba:4d:f0:98:82:a1:0a:e3:3b:1d:d4:c7:5e:20:7a:8d:49:55:92:d5:82:f9:85:2d:0b:7e:01:2c:b0:a4:ff:fe:23:25:04:9b:25:46:69:23:4c:33:e7:24:97:2a:13:d4:26:0b:c8:48:30:9d:84:38:aa:bd:fe:e6:42:e6:a0:48:0a:47:f0:18:4c:fb:e3:ce:fd:43:e9:44:ab:85:2f:ba:61:70:a7:a3:9c:a7:93:3b:a5:f5:90:23:4f:20:fb:57:3e:4c:9d:ac:e8:61:b4:ef:30:2a:0a:b6:33:bc:0b:12:f6:85:1a:e4:48:a8:8d:04:5c:b9:49:a0:b8:91:f1:35:3e:a6:bd:7d:06:c1:af:27:ae:78:6a:b7:9e:2b:d1:9e:a9:b3:57:07:0b:6d:14:f1:5d:57:ab:ed:50:c0:f1:7c:17:de:61:be:2e:af:bc:ab:60:c2:f0:ca:21:77:e6:4f:0f:94:25:74:a4:6d:dd:d9:dd:8d:1d"

### cert.sha1_fingerprint: (string)
Search by leaf certificate's SHA1 fingerprint.

    e.g. ssl.cert.sha1_fingerprint:"4e:aa:aa:fd:d1:d5:b6:7f:e5:a1:f2:df:02:58:11:40:c7:8e:04:73"

### cert.sha256_fingerprint: (string)
Search by leaf certificate's SHA256 fingerprint.

    e.g. ssl.cert.sha256_fingerprint:"df:4a:62:74:eb:16:18:48:0e:2e:da:41:b1:80:f0:d5:62:69:24:6c:38:2b:08:e5:83:26:52:ca:d5:71:2b:ec"

### cert.spki_subject_fingerprint: (string)
Search by leaf certificate's SPKI subject fingerprint.

    e.g. ssl.cert.spki_subject_fingerprint:"d0:0f:ae:7c:ae:5d:c8:b9:37:38:fb:b3:5f:6a:24:cc:e9:51:71:ca:ba:21:3f:73:c5:cd:f6:bc:5b:bf:03:1e"

### cert.subject.common_name: (string)
Search by leaf certificate subject's Common Name.

    e.g. ssl.cert.subject.common_name:microsoft

### cert.subject.organization_name: (string)
Search by leaf certificate subject's Organization Name.

    e.g. ssl.cert.subject.organization_name:microsoft

### cert.subject.organizational_unit_name: (string)
Search by leaf certificate subject's Organizational Unit Name.

    e.g. ssl.cert.subject.organizational_unit_name:microsoft

### cert.subject_names: (string)
Search by leaf certificate subject's names (common_name, organization_name, organizational_unit_name combined).

    e.g. ssl.cert.subject_names:kubernetes

### cert.subject_dns: (string)
Search by leaf certificate subject's DNS (if available).

    e.g. ssl.cert.subject_dns:azure

### cert.extensions.key_usage.*: (boolean)
Search by leaf certificate key usage extension parameters.

    e.g. ssl.cert.extensions.key_usage.digital_signature:true

#### Example parameters

* crl_sign
* data_encipherment
* decipher_only
* digital_signature
* encipher_only
* key_agreement
* key_cert_sign
* key_encipherment
* non_repudiation

### cert.extensions.extended_key_usage.*: (boolean)
Search by leaf certificate extended key usage extension parameters.

    e.g. ssl.cert.extensions.extended_key_usage.server_auth:true

#### Example parameters

* adobe_authentic_documents_trust
* any_extended_key_usage
* capwap_ac
* capwap_wtp
* client_auth
* code_signing
* dvcs
* eap_over_lan
* eap_over_ppp
* email_protection
* ike_intermediate
* ipsec_end_system
* ipsec_ike
* ipsec_tunnel
* ipsec_user
* microsoft_document_signing
* microsoft_efs
* microsoft_efs_recovery
* microsoft_embedded_nt
* microsoft_key_recovery
* microsoft_lifetime_signing
* microsoft_nt5
* microsoft_oem_whql
* microsoft_qualified_subordination
* microsoft_root_list_signer
* microsoft_server_gated
* microsoft_smart_card_logon
* microsoft_time_stamp_signing
* microsoft_trust_list_signing
* microsoft_whql
* ocsp_signing
* piv_content_signing
* pkinit_kpclientauth
* pkinit_kpkdc
* scvp_client
* scvp_server
* secure_shell_client
* secure_shell_server
* send_owner
* send_router
* server_auth
* sip_domain
* time_stamping

### ciphers: (string)
Search by ciphers.

    e.g. ssl.ciphers:TLSV1_2

### client_auth_requirement_string: (string)
Search by whether the client requires auth or not.

    e.g. ssl.server_info.client_auth_requirement_string:"DISABLED"

### highest_ssl_version_supported: (string)
Search by highest SSL version supported.

    e.g. ssl.server_info.highest_ssl_version_supported_string:TLSV1

### ja3: (string)
Search by JA3 fingerprint string:

    e.g. ssl.server_info.ja3:"771,159,0-65281-35"

### ja3_digest: (string)
Search by JA3 fingerprint hash:

    e.g. ssl.server_info.ja3_digest:"8a17b6c8d5c6e1711cb236cc77aaa388"

### ja3_description: (string)
Search by JA3 description:

    e.g. ssl.server_info.ja3_description:nginx

### openssl_cipher_string_supported: (string)
Search by SSL cypher supported.

    e.g. ssl.server_info.openssl_cipher_string_supported:"AES256-SHA"

### tls_wrapped_protocol_string: (string)
Search by TLS protocol string.

    e.g. ssl.server_info.tls_wrapped_protocol_string:"PLAIN_TLS"

### truststores: (string)
Search by SSL truststores.

    e.g. ssl.truststores:mozilla

### compression_name: (string)
Search for Compression name.

    e.g. ssl.vulnerabilities.compression.compression_name:zlib

### supports_compression: (boolean)
Search for Compression support.

    e.g. ssl.vulnerabilities.compression.supports_compression:true

### supports_fallback_scsv: (boolean)
Search for Fallback SCSV support.

    e.g. ssl.vulnerabilities.fallback.supports_fallback_scsv:true

### is_vulnerable_to_heartbleed: (boolean)
Search for Heartbleed.

    e.g. ssl.vulnerabilities.heartbleed.is_vulnerable_to_heartbleed:true

### is_vulnerable_to_ccs_injection: (boolean)
Search for OpenSSL CCS injection.

    e.g. ssl.vulnerabilities.openssl_ccs.is_vulnerable_to_ccs_injection:true

### accepts_client_renegotiation: (boolean)
Search for Renegotiation support.

    e.g. ssl.vulnerabilities.renegotiation.accepts_client_renegotiation:true

### supports_secure_renegotiation: (boolean)
Search for Secure Renegotiation support.

    e.g. ssl.vulnerabilities.renegotiation.supports_secure_renegotiation:true

### robot_result_enum: (string)
Search for ROBOT.

    e.g. ssl.vulnerabilities.robot.robot_result_enum:NOT_VULNERABLE_NO_ORACLE


## Web

### body.content: (string)
Search by HTTP body.

    e.g. web.body.content:bitcoin

### body.sha256: (string)
Search by HTTP body SHA256 fingerprint.

    e.g. web.body.sha256:"a9aa9ec7ef3ec92e7eb52220a9f0cb578ff2ba0a71cb3e9c1a0b828857529fcc"

### body.ssdeep: (string)
Search by HTTP body SSDEEP fingerprint.

    e.g. web.body.ssdeep:"333a484c75636771434d4142623a48535072"

### favicon.md5: (string)
Search by favicon MD5 fingerprint.

    e.g. web.favicon.md5:"a3d6fc11b6c0dc1f43742944823954d3"

### favicon.mmh3: (string)
Search by favicon MMH3 fingerprint.

    e.g. web.favicon.mmh3:"2780979020"

### headers.*: (string)
Search by HTTP headers.

    e.g. web.headers.accept:"json"

#### Available headers
[Search Available Headers](search-web-headers.md)

### path: (string)
Search by HTTP path.

    e.g. web.path:"index.php"

### rendered.content: (string)
Search by rendered HTTP body.

    e.g. web.rendered.content:bitcoin

### rendered.sha256: (string)
Search by rendered HTTP body SHA256 fingerprint.

    e.g. web.rendered.sha256:"a9aa9ec7ef3ec92e7eb52220a9f0cb578ff2ba0a71cb3e9c1a0b828857529fcc"

### rendered.ssdeep: (string)
Search by rendered HTTP body SSDEEP fingerprint.

    e.g. web.rendered.ssdeep:"333a484c75636771434d4142623a48535072"

### server: (string)
Search by HTTP Server header.

    e.g. web.server:apache

### status.code: (int)
Search by HTTP status code.

    e.g. web.status.code:200

### status.message: (string)
Search by HTTP status message.

    e.g. web.status.message:ok

### title: (string)
Search by HTTP title.

    e.g. web.title:amazon

### url: (string)
Search by final url.

    e.g. web.url:"index.php"


## HTTP (deprecated)

### body: (string)
Search by HTTP body.

    e.g. http.body:bitcoin

### header_order: (string)
Search by HTTP header order fingerprint string.

    e.g. http.header_order:"user_agent,host,connection"

### header_order_hash: (string)
Search by HTTP header order fingerprint hash.

    e.g. http.header_order_hash:"ea54a5e969c426b7815aa5540ab4dd93"

### href: (string)
Search by HTTP href.

    e.g. http.href:amazonaws

### httpVersion: (string)
Search by HTTP version.

    e.g. http.httpVersion:"1.1"

### redirects: (string)
Search by HTTP redirects.

    e.g. http.redirects:https

### responseHeaders: (string)
Search by HTTP response headers.

    e.g. http.responseHeaders:google

### server: (string)
Search by HTTP Server header.

    e.g. http.server:apache

### sha256: (string)
Search by SHA256 hash of the body.

    e.g. http.sha256:"066fe13daa5b3416bece7fe09dbf718135908c61f967627a097c3119af0dfa05"

### statusCode: (int)
Search by HTTP status code.

    e.g. http.statusCode:200

### statusMessage: (string)
Search by HTTP status message.

    e.g. http.statusMessage:ok

### title: (string)
Search by HTTP title.

    e.g. http.title:amazon


## MQTT

### auth: (boolean)
Search by whether MQTT has auth enabled or not.

    e.g. mqtt.auth:false

### connected: (boolean)
Search by whether MQTT server was successfully connected to or not.

    e.g. mqtt.connected:true

### num_topics: (int)
Search by MQTT number of topics.

    e.g. mqtt.num_topics:10

### messages: (string)
Search by MQTT messages.

    e.g. mqtt.messages:sms

### protocol: (string)
Search by MQTT protocol (mqtt or mqtts).

    e.g. mqtt.protocol:mqtts

### version: (string)
Search by MQTT protocol version (4 or 5).

    e.g. mqtt.version:4

### topics: (string)
Search by MQTT topics.

    e.g. mqtt.topics:sms


## Kubernetes

### auth_required: (boolean)
Search by whether Kubernates has auth enabled or not.

    e.g. kubernetes.auth_required:false

### connected: (boolean)
Search by whether Kubernates server was successfully connected to or not.

    e.g. kubernetes.connected:true

### pods_names: (string)
Search by Kubernetes pods names.

    e.g. kubernetes.pods_names:credit

### version: (string)
Search by Kubernetes version.

    e.g. kubernetes.version:"1.15"


## RSYNC

### banner: (string)
Search by RSYNC banner.

    e.g. rsync.banner:confidential

### modules.module: (string)
Search by RSYNC module name.

    e.g. rsync.modules.module:release

### modules.status: (string)
Search by RSYNC module status.

    e.g. rsync.modules.status:"@RSYNCD:OK"

### status: (string)
Search by RSYNC status.

    e.g. rsync.status:public

### version: (string)
Search by RSYNC version.

    e.g. rsync.version:"31.0"


## TOR

### exit_node: (boolean)
Search by whether it is a TOR exit node or not.

    e.g. tor.exit_node:true

### first_seen: (date)
Search by date of TOR node first seen.

    e.g.
        tor.first_seen:[2018-09-01 TO 2018-10-01]
        tor.first_seen:2018-09-01

### hostname: (string)
Search by hostname running TOR.

    e.g. tor.hostname:"vultr"

### last_seen: (date)
Search by date of TOR node last seen.

    e.g.
        tor.last_seen:[2018-09-01 TO 2018-10-01]
        tor.last_seen:2018-09-01

### platform: (string)
Search by platform running TOR node.

    e.g. tor.platform:"windows"

### router_name: (string)
Search by TOR router name.

    e.g. tor.router_name:"xenial"


## MongoDB

#### Available search fields

* mongodb.ismaster (boolean)
* mongodb.listDatabases (string)
* mongodb.names (string)
* mongodb.readonly (boolean)
* mongodb.serverInfo (string)
* mongodb.totalSize (int)
* mongodb.version (string)


## ElasticSearch

#### Available search fields

* elasticsearch.build (string)
* elasticsearch.build_flavor (string)
* elasticsearch.build_hash (string)
* elasticsearch.build_type (string)
* elasticsearch.cluster_name (string)
* elasticsearch.cluster_nodes (int)
* elasticsearch.docs (int) - number of documents
* elasticsearch.hostname (string)
* elasticsearch.indices (string) - name of indices
* elasticsearch.indices_raw (string)
* elasticsearch.jvm.version (string)
* elasticsearch.jvm.vm_name (string)
* elasticsearch.jvm.vm_vendor (string)
* elasticsearch.jvm.vm_version (string)
* elasticsearch.modules (string)
* elasticsearch.name (string)
* elasticsearch.node_name (string)
* elasticsearch.os.arch (string)
* elasticsearch.os.cpu.model (string)
* elasticsearch.os.cpu.vendor (string)
* elasticsearch.os.name (string)
* elasticsearch.os.pretty_name (string)
* elasticsearch.os.version (string)
* elasticsearch.plugins (string)
* elasticsearch.roles (string)
* elasticsearch.settings (string)
* elasticsearch.size (int)
* elasticsearch.size_in_bytes (int)
* elasticsearch.total_indexing_buffer (int)
* elasticsearch.version (string)


## Cassandra

#### Available search fields

* cassandra.cluster (string)
* cassandra.cluster_name (string)
* cassandra.cql_version (string)
* cassandra.datacenter (string)
* cassandra.dse (boolean)
* cassandra.dse_version (string)
* cassandra.keyspaces (string)
* cassandra.keyspace_names (string)
* cassandra.rack (string)
* cassandra.table_names (string)
* cassandra.thrift_version (string)
* cassandra.version (string)


## Redis

#### Available search fields

* redis.aof_enabled (string)
* redis.arch_bits (string)
* redis.cluster_enabled (int)
* redis.connected_slaves (int)
* redis.dbs (int)
* redis.keys (int)
* redis.maxmemory (string)
* redis.multiplexing_api (string)
* redis.os (string)
* redis.redis_build_id (string)
* redis.redis_mode (string)
* redis.redis_version (string)
* redis.repl_backlog_size (int)
* redis.role (string)
* redis.stats (string)
* redis.uptime_in_days (int)
* redis.uptime_in_seconds (int)
* redis.used_memory (int)
* redis.used_memory_human (string)
* redis.used_memory_lua (int)
* redis.used_memory_overhead (string)
* redis.used_memory_peak (int)
* redis.used_memory_peak_human (string)
* redis.used_memory_rss (string)
* redis.used_memory_startup (string)
* redis.versions (string)


## Memcached

#### Available search fields

* memcached.bytes (int)
* memcached.pointer_size (int)
* memcached.replication (string)
* memcached.server (string)
* memcached.size (int)
* memcached.total_items (int)
* memcached.uptime (int)
* memcached.version (string)


## RethinkDB

#### Available search fields

* rethinkdb.database_names (string)
* rethinkdb.databases (string)
* rethinkdb.tables_names (string)
