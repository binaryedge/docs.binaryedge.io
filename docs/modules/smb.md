# SMB

Grab SMB information.

## SMB Request Example

```
curl -v -L https://api.binaryedge.io/v1/tasks -d '{"type":"scan", "options":[{"targets":["X.X.X.X"], "ports":[{"port":445, "protocol":"tcp", "modules":["smb"]}]}]}' -H "X-Token:<Token>"
```

## Schema

### SMB Event Schema

```json
{
  ...
  "result": {
    "data": {
      "smb_dialects": [ "string" ],
      "potential_os": "string",
      "cpe": [ "string" ],
      "shares": [{
          "type": "string",
          "name": "string",
          "description": "string"
      }]
    }
  }
}
```

### Contents of the fields

* **smb_dialects** is an array of SMB dialects supported. Must contain at least one entry
* **potentential_os** is the OS that corresponds to the highests supported SMB dialect. Will be empty in the case of only SMB dialect 1.0 being detected
* **cpe** is an array of CPE identifiers for the corresponding desktop and server versions that correlate with the highest supported SMB dialect. Will be empty in the case of only SMB dialect 1.0 being detected
* **shares** is an array of detected shares, which may be empty
  * **type** is the designation of SMB share. Vaules can be: "Disk", "IPC", and "Printer"
  * **name** is the name of the share that is available for connection
  * **description** is a free form string that is populated at the time of share creation. This may be empty

## SMB Event Example

```json
{
  "origin": {
    "type": "smb",
    "job_id": "client-26069164-abe3-4eb4-a65a-e1f524ef0906",
    "client_id": "client",
    "module": "smb",
    "country": "de",
    "ts": 1604438113
  },
  "target": {
      "ip": "10.0.2.4",
      "port": "445",
      "ts": 1604438113,
      "protocol": "tcp",
      "data": {
        "cpe": [
          "cpe:/o:microsoft:windows_10",
          "cpe:/o:microsoft:windows_server_2016"
        ],
        "smb_dialects": [
          "2.0.2",
          "2.1.0",
          "3.0.0",
          "3.0.2",
          "3.1.1"
        ],
        "potential_os": "Windows 10/Windows Server 2016",
        "shares": [
          {
            "type": "Disk",
            "name": "ADMIN$",
            "description": "Remote Admin"
          },
          {
            "type": "Disk",
            "name": "C$",
            "description": "Default share"
          },
          {
            "type": "IPC",
            "name": "IPC$",
            "description": "Remote IPC"
          },
          {
            "type": "Disk",
            "name": "Share1",
            "description": ""
          },
          {
            "type": "Disk",
            "name": "Users",
            "description": ""
          }
        ]
      }
  }
}
```
