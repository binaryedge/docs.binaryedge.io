# SSH

Extract SSH keys and algorithms from SSH servers.

## SSH Request Example

```
curl -v -L https://api.binaryedge.io/v1/tasks -d '{"type":"scan", "options":[{"targets":["X.X.X.X"], "ports":[{"port":22, "protocol":"tcp", "modules":["ssh"]}]}]}' -H "X-Token:<Token>"
```

## Schema

### SSH Event Schema  

```json
{
  ...
  "result": {
    "data": {
      "cyphers": [
        {
          "cypher": "string",
          "key": "string",
          "fingerprint": "string",
          "length": "int | may not be present"
        }
      ],
      "algorithms": {
        "kex": ["string"],
        "server_host_key": ["string"],
        "encryption": ["string"],
        "mac": ["string"],
        "compression": ["string"],
      },
      "banner": "string",
      "hassh": {
        "hassh": "string",
        "hassh_algorithms": "string"
      }
    }
  }
}

```

### Contents of the fields:

* **cyphers** - Array with the cyphers found on the target server, may be empty if the server takes too long to respond.
    * cypher - Name of the cypher
		* key - Public Key
		* fingerprint - Fingerprint of the key
		* length - Length of the key
* **algorithms** - Supported Algorithms that the target server reports.
    * kex - Key Exchange Algorithms
		* server_host_key - Server Host Key Signature Algorithms
		* encryption - Encryption Algorithms
		* mac - Message Authentication Code Algorithms
		* compression - Compression Algorithms
* **banner** - Server response identifying service.
* **hassh** - Network fingerprinting standard which can be used to identify specific Client and Server SSH implementations. See https://github.com/salesforce/hassh for details.
    * hassh - MD5 fingerprint based on the algorithms supported by the server.
    * hassh_algorithms - String used to generate the MD5 fingerprint, containing the algorithms supported by the server.

## SSH Event Example

```json
{
  "origin": {
    "type": "ssh",
    "job_id": "client-816f1185-4bc1-4b5f-9a7d-61a2df315a6b",
    "client_id": "client",
    "country": "uk",
    "module": "grabber",
    "ts": 1453385574412
  },
  "target": {
    "ip": "X.X.X.X",
    "port": 22,
    "protocol": "tcp"
  },
  "result": {
    "data": {
      "cyphers": [
        {
          "cypher": "ecdsa-sha2-nistp256",
          "key": "AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBPO7uB01y7SElA6aQOtX5uIW2pMo7tCoDhQMOyF+OYslkgwiP9nUURvYYT4kkIzmdO200WPTcWVs7KyC7eKHqBs=",
          "fingerprint": "96:40:69:27:c3:79:91:f4:63:ff:f9:4d:4b:8f:32:8c"
        },
        {
          "cypher": "ssh-rsa",
          "key": "AAAAB3NzaC1yc2EAAAABIwAAAQEAudfUFJtWp8R5qPxXB0acGHctH0YyxVrZZfvnG37osNc32kX35aXVm8Ulk49zl/jMIIQnzP7zeOUJeJJsyXsG6Cu3qjLvD5qlc0tRjoVmV08aDgAsfeq7qQFEzzDqyoL8kV9akj8WyP+aN3QHvM4a/+3Y+UTVqrw5jSUiIIW5JOd+UWzSz6SCGalFbop1wGELUTY6MDTHwwn+qXYgltQG6hP5tI9tl3gAVajIHg2IxM8IXz4SYH33ZeOPypzrcr1/DvFx1s0773eGSArIi83BeYyxvN/T68RxIqAieLxVy8zJgyevpqHpUX7/+kDuvVZdfKkmFoNzBTEiIvR5eMrjTw==",
          "fingerprint": "5b:71:c9:85:6a:ea:40:dc:62:95:4c:25:40:b7:97:55",
          "length": 2048
        }
      ],
      "algorithms": {
        "kex": [
          "curve25519-sha256@libssh.org",
          "ecdh-sha2-nistp256",
          "ecdh-sha2-nistp384",
          "ecdh-sha2-nistp521",
          "diffie-hellman-group-exchange-sha256",
          "diffie-hellman-group-exchange-sha1",
          "diffie-hellman-group14-sha1",
          "diffie-hellman-group1-sha1"
        ],
        "server_host_key": [
          "ssh-rsa",
          "ssh-dss",
          "ecdsa-sha2-nistp256",
          "ssh-ed25519"
        ],
        "encryption": [
          "aes128-ctr",
          "aes192-ctr",
          "aes256-ctr",
          "arcfour256",
          "arcfour128",
          "aes128-gcm@openssh.com",
          "aes256-gcm@openssh.com",
          "chacha20-poly1305@openssh.com",
          "aes128-cbc",
          "3des-cbc",
          "blowfish-cbc",
          "cast128-cbc",
          "aes192-cbc",
          "aes256-cbc",
          "arcfour",
          "rijndael-cbc@lysator.liu.se"
        ],
        "mac": [
          "hmac-md5-etm@openssh.com",
          "hmac-sha1-etm@openssh.com",
          "umac-64-etm@openssh.com",
          "umac-128-etm@openssh.com",
          "hmac-sha2-256-etm@openssh.com",
          "hmac-sha2-512-etm@openssh.com",
          "hmac-ripemd160-etm@openssh.com",
          "hmac-sha1-96-etm@openssh.com",
          "hmac-md5-96-etm@openssh.com",
          "hmac-md5",
          "hmac-sha1",
          "umac-64@openssh.com",
          "umac-128@openssh.com",
          "hmac-sha2-256",
          "hmac-sha2-512",
          "hmac-ripemd160",
          "hmac-ripemd160@openssh.com",
          "hmac-sha1-96",
          "hmac-md5-96"
        ],
        "compression": [
          "none",
          "zlib@openssh.com"
        ]
      },
      "banner": "SSH-2.0-OpenSSH_6.6.1p1",
      "hassh": {
        "hassh_algorithms": "curve25519-sha256@libssh.org,ecdh-sha2-nistp256,ecdh-sha2-nistp384,ecdh-sha2-nistp521,diffie-hellman-group-exchange-sha256,diffie-hellman-group-exchange-sha1,diffie-hellman-group14-sha1,diffie-hellman-group1-sha1;aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-gcm@openssh.com,aes256-gcm@openssh.com,chacha20-poly1305@openssh.com,aes128-cbc,3des-cbc,blowfish-cbc,cast128-cbc,aes192-cbc,aes256-cbc,arcfour,rijndael-cbc@lysator.liu.se;hmac-md5-etm@openssh.com,hmac-sha1-etm@openssh.com,umac-64-etm@openssh.com,umac-128-etm@openssh.com,hmac-sha2-256-etm@openssh.com,hmac-sha2-512-etm@openssh.com,hmac-ripemd160-etm@openssh.com,hmac-sha1-96-etm@openssh.com,hmac-md5-96-etm@openssh.com,hmac-md5,hmac-sha1,umac-64@openssh.com,umac-128@openssh.com,hmac-sha2-256,hmac-sha2-512,hmac-ripemd160,hmac-ripemd160@openssh.com,hmac-sha1-96,hmac-md5-96;none,zlib@openssh.com",
        "hassh": "ba6d3d2aecbd0d91b01dfa7828110d70"
      }
    }
  }
}
```
