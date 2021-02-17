# RDPEUDP

Grab RDPEUDP handshake material.

## RDPEUDP Request Example

```
curl -v -L https://api.binaryedge.io/v1/tasks -d '{"type":"grab", "options":[{"targets":["X.X.X.X"], "ports":[{"port":3389, "protocol":"udp", "modules":["rdpeudp"]}]}]}' -H "X-Token:<Token>"
```

## Schema

### RDPEUDP Event Schema

```json
{
  ...
  "result": {
    "data": {
      "byte_response": "string",
      "rdp_transport": "string"
    }
}
```

### Contents of the data fields:

* byte_response- This is the reponse to our request, identifying the protocol
* rdp_tranport- This will always be `udp` for this module

## RDPEUDP Event Example

```json
{
  "target": {
    "ip": "x.x.x.x",
    "port": 3389,
    "protocol": "udp"
  },
  "result": {
    "data": {
      "byte_response": "0x000000000040000598aeca6a04d004d0",
      "rdp_transport": "udp"
    }
  },
  "origin": {
    "ts": 1612490273377,
    "job_id": "22a4a330-d672-4b44-9c8b-9dede6abaf36",
    "ip": "x.x.x.x",
    "module": "grabber",
    "client_id": "client",
    "type": "rdpeudp",
    "minion": "prod-us-scanner-2601-25",
    "country": "us"
  }
}
```