# Sensors Search Parameters

The API has endpoints for querying our data in which you can use free text search together with one or more of the filters listed below.


## Notes

**Free Text**: not specifying a field will search on the full records, which can include other information not stated below. Although free text search without specifying fields is available, it might be processed differently from searching on specific fields. For better results, always specify search fields.

**Conditionals**: the following conditionals are available: NOT, AND, OR. Must be UPPERCASE. You can also use the minus sign (-) as a replacement for the NOT conditional. By default, a sequence of search terms without conditionals will be interpreted as if using the AND conditional. 

**Comparison**: you can use comparison operators on number fields. E.g. _field:>100_.

**Field existence or omission**: you can search for records that have a specific field by using _\_exists\_:field_. Conversely, for records missing a field it would be _NOT \_exists\_:field_.

**Wildcards**: you can use wildcards on your query terms. However, for the best results and performance, try to be as specific as you can. E.g. _field:security*_

**Regular Expressions**: you can use regular expressions on your query terms. For the best results and performance, try to be as specific as you can. If you need to use regular expressions, specify the field you want to search in. E.g. _field:/.*microsoft\..{2}/_

**String fields**: if the string is expected to have spaces, some kind of punctuation in the middle, or special symbols, try quoting the search terms, i.e. instead of querying _field:value_ try _field:"value"_. You can also try instead _field.keyword:"value"_. The first one will search for any occurrence of any of the words in _value_, while the second one will search for an exact match of the string. Finally, in the case of some special symbols, you might require escaping in order to make the query valid.


## Fields

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

### geoip.city_name: (string) 
Search using city names. 
    
    e.g. geoip.city_name:madrid

### geoip.country_name: (string) 
Search using country names. 
    
    e.g. geoip.country_name:spain

### payload: (string) 
Search in the payloads for a specific string
    
    e.g. payload:wget

### port: (int) 
Search by target port number. 
    
    e.g. port:80

### protocol: (string)
Search by protocol. Can be TCP, UDP or ICMP. 
    
    e.g. protocol:tcp

### rdns: (string)
Search by RDNS.

    e.g. rdns:static-206-162-231-219.wireless.unwiredbb.net

### rdns_parent: (string)
Search by RDNS root.

    e.g. rdns_parent:unwiredbb.net

### sha256: (string)
Search by the payload's SHA256 hash.

    e.g. sha256:"db64d98c3a5a540e667f40764a58dfca0df6627a0e34e7d0e8daae097476772d"

### tags: (string)
Search by tags.

    e.g. tags:NMAP_SCANNER

#### Available tags
[Sensors Available Tags](sensors-tags.md)

### extra.http.header_order: (string)
Search by HTTP header order fingerprint string.

    e.g. extra.http.header_order:"user_agent,host,connection"

### extra.http.header_order_hash: (string)
Search by HTTP header order fingerprint hash.

    e.g. extra.http.header_order_hash:"ea54a5e969c426b7815aa5540ab4dd93"

### extra.http.headers.all: (string)
Search by HTTP headers.

    e.g. extra.http.headers.all:google

### extra.http.method: (string)
Search by HTTP method.

    e.g. extra.http.path:POST

### extra.http.path: (string)
Search by HTTP path.

    e.g. extra.http.path:"/index.html"

### extra.ssh.description: (string)
Search by SSH identification string.

    e.g. extra.ssh.description:dropbear

### extra.ssh.hassh: (string)
Search by HASSH fingerprint hash.

    e.g. extra.ssh.hassh:92674389fa1e47a27ddd8d9b63ecd42b

### extra.ssh.hassh_algorithms: (string)
Search by HASSH algorithms fingerprint string.

    e.g. extra.ssh.hassh_algorithms:"diffie-hellman-group14-sha1,diffie-hellman-group-exchange-sha1,diffie-hellman-group1-sha1;aes128-ctr,aes192-ctr,aes256-ctr,aes256-cbc,rijndael-cbc@lysator.liu.se,aes192-cbc,aes128-cbc,blowfish-cbc,arcfour128,arcfour,cast128-cbc,3des-cbc;hmac-sha1,hmac-sha1-96,hmac-md5,hmac-md5-96,hmac-ripemd160,hmac-ripemd160@openssh.com;none"

### extra.ssl.description: (string)
Search by SSL JA3 description.

    e.g. extra.ssl.description:chrome

### extra.ssl.ja3: (string)
Search by SSL JA3 fingerprint string.

    e.g. extra.ssl.ja3:"771,49200-49196-49192-49188-49172-49162-165-163-161-159-107-106-105-104-57-56-55-54-136-135-134-133-49202-49198-49194-49190-49167-49157-157-61-53-132-141-49199-49195-49191-49187-49171-49161-164-162-160-158-103-64-63-62-51-50-49-48-154-153-152-151-69-68-67-66-49201-49197-49193-49189-49166-49156-156-60-47-150-65-140-49169-49159-49164-49154-5-4-138-255,0-11-10-35-13-15-21,23-25-28-27-24-26-22-14-13-11-12-9-10,0-1-2"

### extra.ssl.ja3_digest: (string)
Search by SSL JA3 fingerprint hash.

    e.g. extra.ssl.ja3_digest:85ceddf7e8f7ad69d40c81c05e0fde96

### extra.dtls.description: (string)
Search by DTLS JA3 description.

    e.g. extra.dtls.description:chrome

### extra.dtls.ja3: (string)
Search by DTLS JA3 fingerprint string.

    e.g. extra.dtls.ja3:"771,49200-49196-49192-49188-49172-49162-165-163-161-159-107-106-105-104-57-56-55-54-136-135-134-133-49202-49198-49194-49190-49167-49157-157-61-53-132-141-49199-49195-49191-49187-49171-49161-164-162-160-158-103-64-63-62-51-50-49-48-154-153-152-151-69-68-67-66-49201-49197-49193-49189-49166-49156-156-60-47-150-65-140-49169-49159-49164-49154-5-4-138-255,0-11-10-35-13-15-21,23-25-28-27-24-26-22-14-13-11-12-9-10,0-1-2"

### extra.dtls.ja3_digest: (string)
Search by DTLS JA3 fingerprint hash.

    e.g. extra.dtls.ja3_digest:85ceddf7e8f7ad69d40c81c05e0fde96
