# Image Search Parameters

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

### created_at: (date)
Search by timestamp.

    e.g.
        ts:[2018-09-01 TO 2018-10-01]
        ts:2018-09-01

### country: (string) 
Search using ISO2 Country Codes. 
    
    e.g. country:ES

### ip: (string) 
Search by IP address or CIDR. 

    e.g ip:"192.168.1.1/24" or ip:192.168.1.1

### ipv6: (boolean)
Search for IPv6 results:

    e.g ipv6:true

### geoip.city_name: (string) 
Search using city names. 
    
    e.g. geoip.city_name:madrid

### geoip.country_name: (string) 
Search using country names. 
    
    e.g. geoip.country_name:spain

### port: (int) 
Search by port number. 
    
    e.g. port:80

### protocol: (string)
Search by protocol. Can be TCP or UDP. 
    
    e.g. protocol:tcp

### has_faces: (boolean)
Search for images with faces detected.

    e.g. has_faces:true

### height: (int)
Search by image height (supports ranges).

    e.g. height:1024

### rdns: (string)
Search by RDNS.

    e.g. rdns:static-206-162-231-219.wireless.unwiredbb.net

### rdns_parent: (string)
Search by RDNS root.

    e.g. rdns_parent:unwiredbb.net

### tags: (string)
Search by tags. Can be mobile, rdp, vnc, windows, x11.

    e.g. tags:mobile

#### Available tags

* HAS_FACES
* MOBILE
* RDP
* VNC
* WINDOWS
* X11

### width: (int)
Search by image width (supports ranges).

    e.g. width:768

### words: (string)
Search by text found by OCR.

    e.g. words:alarm
