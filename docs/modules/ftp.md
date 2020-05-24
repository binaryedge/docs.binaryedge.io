# FTP

The FTP module attempts to connect to an FTP server anonymously and recursively list the directories.

## FTP Request Example

```
curl -v -L https://api.binaryedge.io/v1/tasks -d '{"type":"scan", "options":[{"targets":["X.X.X.X"], "ports":[{"port":21, "protocol":"tcp", "modules":["ftp"], "config":{}}]}]}' -H "X-Token:<Token>"
```

### FTP Request Options

These are optional parameters that can alter the behaviour of the module. These options can be inserted into the "config" object on the request.

* ftps - Use FTPS (SSL) instead of plain FTP (can be "implicit" or "explicit")
    * "config":{"ftps":"explicit"}
* max_recursion - Maximum level of recursion when listing folders (default is 3)
    * "config":{"max_recursion":"5"}

## Schema

### FTP Event Schema

```json
{
   "result":{
        "data":{
            "content": [{
                "rights": {
                    "user": "string",
                    "group": "string",
                    "other": "string"
                },
                "owner": "string",
                "size": "int",
                "group": "string",
                "sticky": "boolean",
                "name": "string",
                "acl": "boolean",
                "type": "string",
                "date": "string",
                "content": [
                    ...
                ]
            }],
            "connected": "boolean",
            "password": "string",
            "user": "string",
            "anonymous": "boolean"
        }
   }
   ...
}
```

### Contents of the fields:

* user - username for FTP connection (non configurable, should always be "anonymous")
* password - password for FTP connection (non configurable, should always be "anonymous@")
* anonymous - Whether the anonymous user was used (should always be true)
* connected - Whether the FTP connection was successful
* content - List of files and directories
    * rights - Rights of entry (read, write, execute)
    * owner - Owner of entry
    * group - Group of entry
    * size - Size of entry in bytes
    * sticky - Sticky bit (Unix)
    * name - Name of entry
    * acl - Whether entry has an ACL or not
    * type - Type of entry, file or directory
    * date - Last updated
    * content - If entry is a directory, list of files and directories inside

## FTP Event Example

```json
{
  "result": {
    "data": {
      "content": [
        {
          "rights": {
            "user": "rwx",
            "group": "rwx",
            "other": "rwx"
          },
          "owner": "0",
          "size": 16,
          "group": "0",
          "sticky": false,
          "name": "SS",
          "acl": false,
          "type": "d",
          "date": "2020-03-31T14:30:00.000Z",
          "content": [
            {
              "rights": {
                "user": "rwx",
                "group": "rwx",
                "other": "rwx"
              },
              "owner": "500",
              "size": 4096,
              "group": "1000",
              "sticky": false,
              "name": "BACKUPS",
              "acl": false,
              "type": "d",
              "date": "2020-04-27T09:38:00.000Z"
            },
            {
              "rights": {
                "user": "rwx",
                "group": "rwx",
                "other": "rwx"
              },
              "owner": "500",
              "size": 4096,
              "group": "1000",
              "sticky": false,
              "name": "DATA",
              "acl": false,
              "type": "d",
              "date": "2020-05-17T01:01:00.000Z"
            },
            {
              "rights": {
                "user": "rwx",
                "group": "rwx",
                "other": "rwx"
              },
              "owner": "500",
              "size": 4096,
              "group": "1000",
              "sticky": false,
              "name": "PARTILHA GERAL",
              "acl": false,
              "type": "d",
              "date": "2020-05-22T23:32:00.000Z"
            },
            {
              "rights": {
                "user": "rwx",
                "group": "rwx",
                "other": "rwx"
              },
              "owner": "500",
              "size": 4096,
              "group": "1000",
              "sticky": false,
              "name": "SOFTWARE",
              "acl": false,
              "type": "d",
              "date": "2020-04-27T09:39:00.000Z"
            }
          ]
        }
      ],
      "connected": true,
      "password": "anonymous@",
      "user": "anonymous",
      "anonymous": true
    }
  }
  ...
}
```
