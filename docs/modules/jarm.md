# JARM

The JARM module attempts to actively fingerprint an SSL/TLS server via a series of TLS Client Hello packets to extract specific responses that can be used to quickly identify default applications or malware.

## JARM

```
curl -v -L https://api.binaryedge.io/v1/tasks -d '{"type":"scan", "options":[{"targets":["X.X.X.X"], "ports":[{"port":443, "protocol":"tcp", "modules":["jarm"]}]}]}' -H "X-Token:<Token>"
```

## Schema

### JARM Event Schema

```json
{
  ...
  "result": {
    "data": {
      "jarm": "string",
      "jarm_hash": "string"
    }
  ...
}
```

### Contents of the fields

* jarm - JARM is a method for creating SSL/TLS fingerprints for threat intelligence, based information extracted from the server response to a TLS Client Hello. See https://github.com/salesforce/jarm for details
* jarm_bash - uzzy hash fingerprint using the extracted information on `jarm`

## SSL Simple Event Example

```json
{
  ...
  "result": {
    "data": {
      "jarm": "c02b|0303|h2|0000-0017-ff01-000b-0023-0010,cc14|0303|h2|0000-0017-ff01-000b-0023-0010,cc14|0303|h2|0000-0017-ff01-000b-0023-0010,|||,cc14|0303||0000-0017-ff01-000b-0023,c009|0302|h2|0000-0017-ff01-000b-0023-0010,1302|0303||0033-002b,1303|0303||0033-002b,|||,1301|0303||0033-002b",
      "jarm_hash": "27d3ed3ed0003ed1dc42d43d00041d6183ff1bfae51ebd88d70384363d525c"
    }
  ...
}
```
