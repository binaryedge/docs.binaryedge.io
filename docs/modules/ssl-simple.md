# SSL Simple

The SSL-Simple module attempts to connect to an SSL-wrapped server and extract (and parse) certificate chains. Recommended if you are only interested in certificates, since it is much faster than the sslv2 module as it doesn't need to do any additional testing.

## SSL Simple Request Example

```
curl -v -L https://api.binaryedge.io/v1/tasks -d '{"type":"scan", "options":[{"targets":["X.X.X.X"], "ports":[{"port":443, "protocol":"tcp", "modules":["ssl-simple"]}]}]}' -H "X-Token:<Token>"
```

## Schema

### SSL Simple Event Schema

```json
{
  ...
  "result": {
    "data": {
      "cert_info": {
        "certificate_chain": [
          {
            "as_dict": {
              "extensions": {},
              "serial_number": "string",
              "subject": {
                "common_name": "string",
                "locality_name": "string",
                "organization_name": "string",
                "organizational_unit_name": "string",
                "country_name": "string",
                "state_or_province_name": "string"
              },
              "public_key_info": {
                "algorithm": "string",
                "key_size": "string",
                "modulus": "string",
                "exponent": "string",
                "curve": "string",
                "public_key": "string",
                "p": "string",
                "q": "string",
                "g": "string"
              },
              "validity": {
                "not_after": "string",
                "not_before": "string"
              },
              "version": "int",
              "issuer": {
                "common_name": "string",
                "locality_name": "string",
                "organizational_unit_name": "string",
                "organization_name": "string",
                "country_name": "string",
                "state_or_province_name": "string"
              },
              "signature_algorithm": "string",
              "signature_value": "string"
            },
            "sha1_fingerprint": "string",
            "sha256_fingerprint": "string",
            "as_pem": "string"
          },
          ...
        ],
      },
      "server_info": {
        "openssl_cipher_string_supported": "string",
        "hostname": "string",
        "highest_ssl_version_supported": "string",
        "port": "int",
        "ip_address": "string",
        "ja3": "string",
        "ja3_digest": "string"
      }
    }
  },
  ...
}
```

### Contents of the fields

* cert_info - verify the validity of the server(s) certificate(s)
    * certificate_chain - the certificate chain sent by the server; index 0 is the leaf certificate
        * as_dict
            * extensions - contains the target's certificate extensions information
            * serial_number - the certificate serial number
            * subject - subject contains the target's certificate subject information
                * common_name - common name of the subject
                * locality_name - locality of the subject
                * organization_name - organization name of the subject
                * organizational_unit_name - organizational unit name of the subject
                * country_name - country of the subject
                * state_or_province_name - state or province of the subject
            * public_key_info - contains information about the public key stored in the certificate
                * algorithm - algorithm used to create the public key
                * key_size - size of the public key
                * modulus - returns the value of attribute modulus (RSA)
                * exponent - returns the value of attribute exponent (RSA)
                * curve - returns the curve used to create the public key (EC)
                * p - returns the value of attribute p (DSA)
                * q - returns the value of attribute q (DSA)
                * g - returns the value of attribute g (DSA)
                * public_key - contains the target public key (DSA,EC)
            * validity -  contains the target's certificate validity
                * not_after - expiration date of the certificate
                * not_before - date from which the certificate is valid
            * version - the certificate SSL version
            * issuer - contains the target's certificate issuer information
                * common_name - common name of the issuer
                * locality_name - locality of the issuer
                * organization_name - organization name of the issuer
                * organizational_unit_name - organizational name of the issuer
                * country_name - country of the issuer
                * state_or_province_name - stae or province of the issuer
            * signature_algorithm - the certificate signature algorithm
            * signature_value - the certificate signature
        * sha1_fingerprint - the SHA1 fingerprint of the certificate
        * sha256_fingerprint - the SHA256 fingerprint of the certificate
        * as_pem - the certificate in PEM format
* server_info - the server against which the command was run
    * openssl_cipher_string_supported - one of the ssl ciphers supported by the server
    * hostname - the server's hostname
    * highest_ssl_version_supported - the highest version of ssl supported for connections
    * port - the server's TLS port number
    * ip_address - the server's IP address. If not supplied, a DNS lookup for the specified `hostname` will be performed.
    * ja3 - JA3 is a method for creating SSL/TLS fingerprints for threat intelligence, based on version, cyphers and extensions supported by the server. See https://github.com/salesforce/ja3 for details
    * ja3_digest - MD5 fingerprint using the extracted information on `ja3`

## SSL Simple Event Example

```json
{
  ...
  "result": {
    "data": {
      "server_info": {
        "hostname": "binaryedge.io",
        "ip_address": "104.28.6.147",
        "port": 443,
        "highest_ssl_version_supported": "TLSv1.2",
        "openssl_cipher_string_supported": "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256"
      },
      "cert_info": {
        "certificate_chain": [
          {
            "as_pem": "-----BEGIN CERTIFICATE-----\nMIIIUTCCB/egAwIBAgIQIQLt8/npbdZy4Xw1mwSd3zAKBggqhkjOPQQDAjCBkjELMAkGA1UEBhMCR0IxGzAZBgNVBAgTEkdyZWF0ZXIgTWFuY2hlc3RlcjEQMA4GA1UEBxMHU2FsZm9yZDEaMBgGA1UEChMRQ09NT0RPIENBIExpbWl0ZWQxODA2BgNVBAMTL0NPTU9ETyBFQ0MgRG9tYWluIFZhbGlkYXRpb24gU2VjdXJlIFNlcnZlciBDQSAyMB4XDTE5MDkyODAwMDAwMFoXDTIwMDQwNTIzNTk1OVowbDEhMB8GA1UECxMYRG9tYWluIENvbnRyb2wgVmFsaWRhdGVkMSEwHwYDVQQLExhQb3NpdGl2ZVNTTCBNdWx0aS1Eb21haW4xJDAiBgNVBAMTG3NuaTE3NzUyOC5jbG91ZGZsYXJlc3NsLmNvbTBZMBMGByqGSM49AgEGCCqGSM49AwEHA0IABOWOa3A0++wfMHhWZATKN/99EvxV5ZPzyYVMDuRAI+RvIMCle6mdkuql12OuE3S8V4hgAdKWbBUwM7O4rq/4F++jggZSMIIGTjAfBgNVHSMEGDAWgBRACWFn8LyDcU/eEggsb9TUK3Y9ljAdBgNVHQ4EFgQUKPuHno33C/KtLHm3+Dh1EwveCvAwDgYDVR0PAQH/BAQDAgeAMAwGA1UdEwEB/wQCMAAwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsGAQUFBwMCME8GA1UdIARIMEYwOgYLKwYBBAGyMQECAgcwKzApBggrBgEFBQcCARYdaHR0cHM6Ly9zZWN1cmUuY29tb2RvLmNvbS9DUFMwCAYGZ4EMAQIBMFYGA1UdHwRPME0wS6BJoEeGRWh0dHA6Ly9jcmwuY29tb2RvY2E0LmNvbS9DT01PRE9FQ0NEb21haW5WYWxpZGF0aW9uU2VjdXJlU2VydmVyQ0EyLmNybDCBiAYIKwYBBQUHAQEEfDB6MFEGCCsGAQUFBzAChkVodHRwOi8vY3J0LmNvbW9kb2NhNC5jb20vQ09NT0RPRUNDRG9tYWluVmFsaWRhdGlvblNlY3VyZVNlcnZlckNBMi5jcnQwJQYIKwYBBQUHMAGGGWh0dHA6Ly9vY3NwLmNvbW9kb2NhNC5jb20wggOSBgNVHREEggOJMIIDhYIbc25pMTc3NTI4LmNsb3VkZmxhcmVzc2wuY29tggkqLjQwZnkuaW+CCSouYXRscy50b4IPKi5iaW5hcnllZGdlLmlvggwqLmJvZXlydG4uY2aCDyouY29kZWF2ZW51ZS5sa4IQKi5jb250ZW50LnNjaG9vbIIWKi5jb3p5c2xlZXBvdXRkb29yLmNvbYIMKi5kbWR2ZXJpLnJ1ghUqLmhhcGhhemFyZGdvdXJtZXQubWyCESouaG9tZXJvb20uc2Nob29sghQqLmhvb2tlZG9uY291bnRyeS5tbIIJKi5penNlLmNmghAqLmxvYWdxbnJhbmNoLm1sgiAqLnBpenplcmlhLWFsZnJlZG8tb2JlcmhhdXNlbi5kZYIUKi5wb3J0Y2l0eXNwb3J0cy5uZXSCEioucHVibGlzaGVyLnNjaG9vbIIUKi5zZXJpZWFtb25hbW91ci5jb22CECouc3AtaW1wb3J0cy5jb22CCyouc3BsaWZlLnJ1gg0qLnRscmllaGxlLm1sgg0qLnRyb3lzaGF3Lm1sghIqLnZpc2l0c3Rsb3Vpcy5jb22CEyoud29yZHBvd2VyZWQuY28udWuCFSoud29yZHByZXNzLXNlby5jby51a4IRKi55dWdvLWdyYWRueWEucnWCBzQwZnkuaW+CB2F0bHMudG+CDWJpbmFyeWVkZ2UuaW+CCmJvZXlydG4uY2aCDWNvZGVhdmVudWUubGuCDmNvbnRlbnQuc2Nob29sghRjb3p5c2xlZXBvdXRkb29yLmNvbYIKZG1kdmVyaS5ydYITaGFwaGF6YXJkZ291cm1ldC5tbIIPaG9tZXJvb20uc2Nob29sghJob29rZWRvbmNvdW50cnkubWyCB2l6c2UuY2aCDmxvYWdxbnJhbmNoLm1sgh5waXp6ZXJpYS1hbGZyZWRvLW9iZXJoYXVzZW4uZGWCEnBvcnRjaXR5c3BvcnRzLm5ldIIQcHVibGlzaGVyLnNjaG9vbIISc2VyaWVhbW9uYW1vdXIuY29tgg5zcC1pbXBvcnRzLmNvbYIJc3BsaWZlLnJ1ggt0bHJpZWhsZS5tbIILdHJveXNoYXcubWyCEHZpc2l0c3Rsb3Vpcy5jb22CEXdvcmRwb3dlcmVkLmNvLnVrghN3b3JkcHJlc3Mtc2VvLmNvLnVrgg95dWdvLWdyYWRueWEucnUwggEDBgorBgEEAdZ5AgQCBIH0BIHxAO8AdgCyHgXMi6LNiiBOh2b5K7mKJSBna9r6cOeySVMt74uQXgAAAW12fWsQAAAEAwBHMEUCIQDHNdAmRVbj4C9s0IjCg9JVtIfjeGQWjSTh4jepfZ5R0AIgYAE9DsATmGz7jBMQHaUCHv/axsp9W/ioduMnnF38n+wAdQBep3P531bA57U2SH3QSeAyepGaDIShEhKEGHWWgXFFWAAAAW12fWsyAAAEAwBGMEQCICsGwzpybL3/e000pyO4qOiUm/v9Cjiz24E9+Buul454AiBoSMHmyLZmsoJhK/GdnaClqHPKckXyTIM4fivClRZssjAKBggqhkjOPQQDAgNIADBFAiEAznDhrr3ekXuhGEcr64lIyEYl/8xQVv3Jh52myS8xKzACIDjr/wA8OiVtK6vN6wokI/O/AQow/k6NpwzuVunv3z5B\n-----END CERTIFICATE-----\n",
            "sha1_fingerprint": "eb:33:e4:27:37:98:99:71:4d:54:23:72:dc:8a:09:72:ae:8f:cd:e3",
            "sha256_fingerprint": "35:01:8f:1d:b8:ba:aa:12:70:41:94:53:f7:7c:d7:7a:fb:48:90:f5:0d:d6:51:bc:57:76:d3:eb:c0:78:c2:65",
            "as_dict": {
              "signature_algorithm": "sha256_ecdsa",
              "signature_value": "30:45:02:21:00:ce:70:e1:ae:bd:de:91:7b:a1:18:47:2b:eb:89:48:c8:46:25:ff:cc:50:56:fd:c9:87:9d:a6:c9:2f:31:2b:30:02:20:38:eb:ff:00:3c:3a:25:6d:2b:ab:cd:eb:0a:24:23:f3:bf:01:0a:30:fe:4e:8d:a7:0c:ee:56:e9:ef:df:3e:41",
              "issuer": {
                "country_name": "GB",
                "state_or_province_name": "Greater Manchester",
                "locality_name": "Salford",
                "organization_name": "COMODO CA Limited",
                "common_name": "COMODO ECC Domain Validation Secure Server CA 2",
                "distinguished_name": "Common Name: COMODO ECC Domain Validation Secure Server CA 2, Organization: COMODO CA Limited, Locality: Salford, State/Province: Greater Manchester, Country: GB"
              },
              "subject": {
                "organizational_unit_name": "Domain Control Validated | PositiveSSL Multi-Domain",
                "common_name": "sni177528.cloudflaressl.com",
                "distinguished_name": "Common Name: sni177528.cloudflaressl.com; Organizational Unit: PositiveSSL Multi-Domain, Domain Control Validated"
              },
              "validity": {
                "not_after": "2020-04-05T23:59:59+00:00",
                "not_before": "2019-09-28T00:00:00+00:00"
              },
              "serial_number": "43879734715482815728520234146753453535",
              "version": "v3",
              "public_key_info": {
                "algorithm": "ec",
                "curve": "secp256r1",
                "public_key": "04:e5:8e:6b:70:34:fb:ec:1f:30:78:56:64:04:ca:37:ff:7d:12:fc:55:e5:93:f3:c9:85:4c:0e:e4:40:23:e4:6f:20:c0:a5:7b:a9:9d:92:ea:a5:d7:63:ae:13:74:bc:57:88:60:01:d2:96:6c:15:30:33:b3:b8:ae:af:f8:17:ef",
                "key_size": 256,
                "sha256_fingerprint": "21:a5:e8:c4:61:97:08:3f:2a:cc:bb:bd:d9:1d:6c:d9:a8:8e:fc:f9:43:3f:ae:ce:10:48:41:71:d0:cb:54:a3"
              },
              "extensions": {
                "authority_key_identifier": {
                  "key_identifier": "40:09:61:67:f0:bc:83:71:4f:de:12:08:2c:6f:d4:d4:2b:76:3d:96"
                },
                "key_identifier": "28:fb:87:9e:8d:f7:0b:f2:ad:2c:79:b7:f8:38:75:13:0b:de:0a:f0",
                "key_usage": [
                  "digital_signature"
                ],
                "basic_constraints": "",
                "extended_key_usage": [
                  "server_auth",
                  "client_auth"
                ],
                "certificate_policies": [
                  {
                    "policy_identifier": "1.3.6.1.4.1.6449.1.2.2.7",
                    "policy_qualifiers": [
                      {
                        "policy_qualifier_id": "certification_practice_statement",
                        "qualifier": "https://secure.comodo.com/CPS"
                      }
                    ]
                  },
                  {
                    "policy_identifier": "2.23.140.1.2.1"
                  }
                ],
                "crl_distribution_points": [
                  {
                    "distribution_point": [
                      "http://crl.comodoca4.com/COMODOECCDomainValidationSecureServerCA2.crl"
                    ]
                  }
                ],
                "authority_information_access": [
                  {
                    "access_method": "ca_issuers",
                    "access_location": "http://crt.comodoca4.com/COMODOECCDomainValidationSecureServerCA2.crt"
                  },
                  {
                    "access_method": "ocsp",
                    "access_location": "http://ocsp.comodoca4.com"
                  }
                ],
                "subject_alt_name": [
                  "sni177528.cloudflaressl.com",
                  "*.40fy.io",
                  "*.atls.to",
                  "*.binaryedge.io",
                  "*.boeyrtn.cf",
                  "*.codeavenue.lk",
                  "*.content.school",
                  "*.cozysleepoutdoor.com",
                  "*.dmdveri.ru",
                  "*.haphazardgourmet.ml",
                  "*.homeroom.school",
                  "*.hookedoncountry.ml",
                  "*.izse.cf",
                  "*.loagqnranch.ml",
                  "*.pizzeria-alfredo-oberhausen.de",
                  "*.portcitysports.net",
                  "*.publisher.school",
                  "*.serieamonamour.com",
                  "*.sp-imports.com",
                  "*.splife.ru",
                  "*.tlriehle.ml",
                  "*.troyshaw.ml",
                  "*.visitstlouis.com",
                  "*.wordpowered.co.uk",
                  "*.wordpress-seo.co.uk",
                  "*.yugo-gradnya.ru",
                  "40fy.io",
                  "atls.to",
                  "binaryedge.io",
                  "boeyrtn.cf",
                  "codeavenue.lk",
                  "content.school",
                  "cozysleepoutdoor.com",
                  "dmdveri.ru",
                  "haphazardgourmet.ml",
                  "homeroom.school",
                  "hookedoncountry.ml",
                  "izse.cf",
                  "loagqnranch.ml",
                  "pizzeria-alfredo-oberhausen.de",
                  "portcitysports.net",
                  "publisher.school",
                  "serieamonamour.com",
                  "sp-imports.com",
                  "splife.ru",
                  "tlriehle.ml",
                  "troyshaw.ml",
                  "visitstlouis.com",
                  "wordpowered.co.uk",
                  "wordpress-seo.co.uk",
                  "yugo-gradnya.ru"
                ],
                "signed_certificate_timestamp_list": {
                  "packed": "00:ef:00:76:00:b2:1e:05:cc:8b:a2:cd:8a:20:4e:87:66:f9:2b:b9:8a:25:20:67:6b:da:fa:70:e7:b2:49:53:2d:ef:8b:90:5e:00:00:01:6d:76:7d:6b:10:00:00:04:03:00:47:30:45:02:21:00:c7:35:d0:26:45:56:e3:e0:2f:6c:d0:88:c2:83:d2:55:b4:87:e3:78:64:16:8d:24:e1:e2:37:a9:7d:9e:51:d0:02:20:60:01:3d:0e:c0:13:98:6c:fb:8c:13:10:1d:a5:02:1e:ff:da:c6:ca:7d:5b:f8:a8:76:e3:27:9c:5d:fc:9f:ec:00:75:00:5e:a7:73:f9:df:56:c0:e7:b5:36:48:7d:d0:49:e0:32:7a:91:9a:0c:84:a1:12:12:84:18:75:96:81:71:45:58:00:00:01:6d:76:7d:6b:32:00:00:04:03:00:46:30:44:02:20:2b:06:c3:3a:72:6c:bd:ff:7b:4d:34:a7:23:b8:a8:e8:94:9b:fb:fd:0a:38:b3:db:81:3d:f8:1b:ae:97:8e:78:02:20:68:48:c1:e6:c8:b6:66:b2:82:61:2b:f1:9d:9d:a0:a5:a8:73:ca:72:45:f2:4c:83:38:7e:2b:c2:95:16:6c:b2",
                  "unpacked": [
                    {
                      "version": "v1",
                      "log_id": "b2:1e:05:cc:8b:a2:cd:8a:20:4e:87:66:f9:2b:b9:8a:25:20:67:6b:da:fa:70:e7:b2:49:53:2d:ef:8b:90:5e",
                      "timestamp": "2019-09-28T07:09:53.936000",
                      "signature_algorithm": "sha256_ecdsa",
                      "signature": "30:45:02:21:00:c7:35:d0:26:45:56:e3:e0:2f:6c:d0:88:c2:83:d2:55:b4:87:e3:78:64:16:8d:24:e1:e2:37:a9:7d:9e:51:d0:02:20:60:01:3d:0e:c0:13:98:6c:fb:8c:13:10:1d:a5:02:1e:ff:da:c6:ca:7d:5b:f8:a8:76:e3:27:9c:5d:fc:9f:ec"
                    },
                    {
                      "version": "v1",
                      "log_id": "5e:a7:73:f9:df:56:c0:e7:b5:36:48:7d:d0:49:e0:32:7a:91:9a:0c:84:a1:12:12:84:18:75:96:81:71:45:58",
                      "timestamp": "2019-09-28T07:09:53.970000",
                      "signature_algorithm": "sha256_ecdsa",
                      "signature": "30:44:02:20:2b:06:c3:3a:72:6c:bd:ff:7b:4d:34:a7:23:b8:a8:e8:94:9b:fb:fd:0a:38:b3:db:81:3d:f8:1b:ae:97:8e:78:02:20:68:48:c1:e6:c8:b6:66:b2:82:61:2b:f1:9d:9d:a0:a5:a8:73:ca:72:45:f2:4c:83:38:7e:2b:c2:95:16:6c:b2"
                    }
                  ]
                }
              },
              "self_issued": false,
              "self_signed": false
            },
            "spki_subject_fingerprint": "87:33:f9:7b:5e:3c:95:1c:41:2a:93:f7:14:c6:0a:3f:87:b3:dd:da:7c:7b:1e:e0:08:a4:66:5d:f3:8d:29:3c"
          },
          {
            "as_pem": "-----BEGIN CERTIFICATE-----\nMIIDnzCCAyWgAwIBAgIQWyXOaQfEJlVm0zkMmalUrTAKBggqhkjOPQQDAzCBhTELMAkGA1UEBhMCR0IxGzAZBgNVBAgTEkdyZWF0ZXIgTWFuY2hlc3RlcjEQMA4GA1UEBxMHU2FsZm9yZDEaMBgGA1UEChMRQ09NT0RPIENBIExpbWl0ZWQxKzApBgNVBAMTIkNPTU9ETyBFQ0MgQ2VydGlmaWNhdGlvbiBBdXRob3JpdHkwHhcNMTQwOTI1MDAwMDAwWhcNMjkwOTI0MjM1OTU5WjCBkjELMAkGA1UEBhMCR0IxGzAZBgNVBAgTEkdyZWF0ZXIgTWFuY2hlc3RlcjEQMA4GA1UEBxMHU2FsZm9yZDEaMBgGA1UEChMRQ09NT0RPIENBIExpbWl0ZWQxODA2BgNVBAMTL0NPTU9ETyBFQ0MgRG9tYWluIFZhbGlkYXRpb24gU2VjdXJlIFNlcnZlciBDQSAyMFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEAjgZgTrJaYRwWQKOqIofMN+83gP8eR06JSxrQSEYgur5PkrkM8wSzypD/A7yZADA4SVQgiTNtkk4DyVHkUikraOCAWYwggFiMB8GA1UdIwQYMBaAFHVxpxlIGbydnepBR9+UxEh3mdN5MB0GA1UdDgQWBBRACWFn8LyDcU/eEggsb9TUK3Y9ljAOBgNVHQ8BAf8EBAMCAYYwEgYDVR0TAQH/BAgwBgEB/wIBADAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUHAwIwGwYDVR0gBBQwEjAGBgRVHSAAMAgGBmeBDAECATBMBgNVHR8ERTBDMEGgP6A9hjtodHRwOi8vY3JsLmNvbW9kb2NhLmNvbS9DT01PRE9FQ0NDZXJ0aWZpY2F0aW9uQXV0aG9yaXR5LmNybDByBggrBgEFBQcBAQRmMGQwOwYIKwYBBQUHMAKGL2h0dHA6Ly9jcnQuY29tb2RvY2EuY29tL0NPTU9ET0VDQ0FkZFRydXN0Q0EuY3J0MCUGCCsGAQUFBzABhhlodHRwOi8vb2NzcC5jb21vZG9jYTQuY29tMAoGCCqGSM49BAMDA2gAMGUCMQCsaEclgBNPE1bAojcJl1pQxOfttGHLKIoKETKm4nHfEQGJbwd6IGZrGNC5LkP3Um8CMBKFfI4TZpIEuppFCZRKMGHRSdxv6+ctyYnPHmp87IXOMCVZuoFwNLg0f+cB0eLLUg==\n-----END CERTIFICATE-----\n",
            "sha1_fingerprint": "75:cf:d9:bc:5c:ef:a1:04:ec:c1:08:2d:77:e6:33:92:cc:ba:52:91",
            "sha256_fingerprint": "cd:6c:10:8a:0e:64:1f:2c:a1:22:aa:a6:d0:3f:82:67:59:ca:e7:c6:f8:00:ea:bf:76:dc:48:b6:7c:d0:83:ce",
            "as_dict": {
              "signature_algorithm": "sha384_ecdsa",
              "signature_value": "30:65:02:31:00:ac:68:47:25:80:13:4f:13:56:c0:a2:37:09:97:5a:50:c4:e7:ed:b4:61:cb:28:8a:0a:11:32:a6:e2:71:df:11:01:89:6f:07:7a:20:66:6b:18:d0:b9:2e:43:f7:52:6f:02:30:12:85:7c:8e:13:66:92:04:ba:9a:45:09:94:4a:30:61:d1:49:dc:6f:eb:e7:2d:c9:89:cf:1e:6a:7c:ec:85:ce:30:25:59:ba:81:70:34:b8:34:7f:e7:01:d1:e2:cb:52",
              "issuer": {
                "country_name": "GB",
                "state_or_province_name": "Greater Manchester",
                "locality_name": "Salford",
                "organization_name": "COMODO CA Limited",
                "common_name": "COMODO ECC Certification Authority",
                "distinguished_name": "Common Name: COMODO ECC Certification Authority, Organization: COMODO CA Limited, Locality: Salford, State/Province: Greater Manchester, Country: GB"
              },
              "subject": {
                "country_name": "GB",
                "state_or_province_name": "Greater Manchester",
                "locality_name": "Salford",
                "organization_name": "COMODO CA Limited",
                "common_name": "COMODO ECC Domain Validation Secure Server CA 2",
                "distinguished_name": "Common Name: COMODO ECC Domain Validation Secure Server CA 2, Organization: COMODO CA Limited, Locality: Salford, State/Province: Greater Manchester, Country: GB"
              },
              "validity": {
                "not_after": "2029-09-24T23:59:59+00:00",
                "not_before": "2014-09-25T00:00:00+00:00"
              },
              "serial_number": "121156049097932074853067657954953090221",
              "version": "v3",
              "public_key_info": {
                "algorithm": "ec",
                "curve": "secp256r1",
                "public_key": "04:02:38:19:81:3a:c9:69:84:70:59:02:8e:a8:8a:1f:30:df:bc:de:03:fc:79:1d:3a:25:2c:6b:41:21:18:82:ea:f9:3e:4a:e4:33:cc:12:cf:2a:43:fc:0e:f2:64:00:c0:e1:25:50:82:24:cd:b6:49:38:0f:25:47:91:48:a4:ad",
                "key_size": 256,
                "sha256_fingerprint": "c7:d4:99:c3:a4:f0:22:a7:e6:be:b2:d9:fe:4c:f5:a3:43:ac:b2:39:a7:ef:6f:01:9c:12:a9:50:5a:86:35:53"
              },
              "extensions": {
                "authority_key_identifier": {
                  "key_identifier": "75:71:a7:19:48:19:bc:9d:9d:ea:41:47:df:94:c4:48:77:99:d3:79"
                },
                "key_identifier": "40:09:61:67:f0:bc:83:71:4f:de:12:08:2c:6f:d4:d4:2b:76:3d:96",
                "key_usage": [
                  "digital_signature",
                  "key_cert_sign",
                  "crl_sign"
                ],
                "basic_constraints": {
                  "ca": true
                },
                "extended_key_usage": [
                  "server_auth",
                  "client_auth"
                ],
                "certificate_policies": [
                  {
                    "policy_identifier": "any_policy"
                  },
                  {
                    "policy_identifier": "2.23.140.1.2.1"
                  }
                ],
                "crl_distribution_points": [
                  {
                    "distribution_point": [
                      "http://crl.comodoca.com/COMODOECCCertificationAuthority.crl"
                    ]
                  }
                ],
                "authority_information_access": [
                  {
                    "access_method": "ca_issuers",
                    "access_location": "http://crt.comodoca.com/COMODOECCAddTrustCA.crt"
                  },
                  {
                    "access_method": "ocsp",
                    "access_location": "http://ocsp.comodoca4.com"
                  }
                ]
              },
              "self_issued": false,
              "self_signed": false
            },
            "spki_subject_fingerprint": "4c:ff:37:4b:fe:bf:62:04:72:bc:1b:99:83:b1:69:cf:62:86:ce:8e:1e:b5:8b:02:7c:f1:40:ac:91:75:4c:96"
          },
          {
            "as_pem": "-----BEGIN CERTIFICATE-----\nMIID0DCCArigAwIBAgIQQ1ICP/qokB8Tn+P05cFETjANBgkqhkiG9w0BAQwFADBvMQswCQYDVQQGEwJTRTEUMBIGA1UEChMLQWRkVHJ1c3QgQUIxJjAkBgNVBAsTHUFkZFRydXN0IEV4dGVybmFsIFRUUCBOZXR3b3JrMSIwIAYDVQQDExlBZGRUcnVzdCBFeHRlcm5hbCBDQSBSb290MB4XDTAwMDUzMDEwNDgzOFoXDTIwMDUzMDEwNDgzOFowgYUxCzAJBgNVBAYTAkdCMRswGQYDVQQIExJHcmVhdGVyIE1hbmNoZXN0ZXIxEDAOBgNVBAcTB1NhbGZvcmQxGjAYBgNVBAoTEUNPTU9ETyBDQSBMaW1pdGVkMSswKQYDVQQDEyJDT01PRE8gRUNDIENlcnRpZmljYXRpb24gQXV0aG9yaXR5MHYwEAYHKoZIzj0CAQYFK4EEACIDYgAEA0d7L3XJghWF+3XkkRbUq2KZ9T5SCwbOQQB/l+EKJDwdAQTuPdKNCZcM4HXk+vt3iir1A2BLNosWIxatCXH0SvQoULT+iBxuP2wvLwlZW6VbCzOZ4sM9iflqLO+y0wbpo4H+MIH7MB8GA1UdIwQYMBaAFK29mHo0tCb3+sQmVO8DveAky1QaMB0GA1UdDgQWBBR1cacZSBm8nZ3qQUfflMRId5nTeTAOBgNVHQ8BAf8EBAMCAYYwDwYDVR0TAQH/BAUwAwEB/zARBgNVHSAECjAIMAYGBFUdIAAwSQYDVR0fBEIwQDA+oDygOoY4aHR0cDovL2NybC50cnVzdC1wcm92aWRlci5jb20vQWRkVHJ1c3RFeHRlcm5hbENBUm9vdC5jcmwwOgYIKwYBBQUHAQEELjAsMCoGCCsGAQUFBzABhh5odHRwOi8vb2NzcC50cnVzdC1wcm92aWRlci5jb20wDQYJKoZIhvcNAQEMBQADggEBAB3H+i5AtlwFSw+8VTYBWOBTBT1k+6zZpTi4pyE7r5VbvkjI00PUIWxB7QktnHMAcZyuIXN+/46NuY5YkI78jG12yAA6nyCmLX3MF/3NmJYyCRrJZfwE67SaCnjllztSjxLCdJcBns/hbWjYk7mcJPuWJ0gBnOqUP3CYQbNzUTcp6PYBerknuCRR2RFo1KaFpzanpZa6gPim/a5thCCuNXZzQg+HCezF3OeTAyIal+6ailFhp5cmHunudVEIkAWvL54TnJM/ev/m6+loeYyv4Lb67psSE/5FjNJ80zXrIRKT/mZ1JioVhCb3ZsnLjbsJQdQYr7GzEPUQyp2aDrV1aug=\n-----END CERTIFICATE-----\n",
            "sha1_fingerprint": "ae:22:3c:bf:20:19:1b:40:d7:ff:b4:ea:57:01:b6:5f:dc:68:a1:ca",
            "sha256_fingerprint": "95:73:86:2a:c0:b4:b1:25:16:88:10:ea:3f:d1:01:ae:2e:b0:bb:15:f6:1f:c0:e6:da:7a:2a:38:b8:5a:89:e8",
            "as_dict": {
              "signature_algorithm": "sha384_rsa",
              "signature_value": "1d:c7:fa:2e:40:b6:5c:05:4b:0f:bc:55:36:01:58:e0:53:05:3d:64:fb:ac:d9:a5:38:b8:a7:21:3b:af:95:5b:be:48:c8:d3:43:d4:21:6c:41:ed:09:2d:9c:73:00:71:9c:ae:21:73:7e:ff:8e:8d:b9:8e:58:90:8e:fc:8c:6d:76:c8:00:3a:9f:20:a6:2d:7d:cc:17:fd:cd:98:96:32:09:1a:c9:65:fc:04:eb:b4:9a:0a:78:e5:97:3b:52:8f:12:c2:74:97:01:9e:cf:e1:6d:68:d8:93:b9:9c:24:fb:96:27:48:01:9c:ea:94:3f:70:98:41:b3:73:51:37:29:e8:f6:01:7a:b9:27:b8:24:51:d9:11:68:d4:a6:85:a7:36:a7:a5:96:ba:80:f8:a6:fd:ae:6d:84:20:ae:35:76:73:42:0f:87:09:ec:c5:dc:e7:93:03:22:1a:97:ee:9a:8a:51:61:a7:97:26:1e:e9:ee:75:51:08:90:05:af:2f:9e:13:9c:93:3f:7a:ff:e6:eb:e9:68:79:8c:af:e0:b6:fa:ee:9b:12:13:fe:45:8c:d2:7c:d3:35:eb:21:12:93:fe:66:75:26:2a:15:84:26:f7:66:c9:cb:8d:bb:09:41:d4:18:af:b1:b3:10:f5:10:ca:9d:9a:0e:b5:75:6a:e8",
              "issuer": {
                "country_name": "SE",
                "organization_name": "AddTrust AB",
                "organizational_unit_name": "AddTrust External TTP Network",
                "common_name": "AddTrust External CA Root",
                "distinguished_name": "Common Name: AddTrust External CA Root, Organizational Unit: AddTrust External TTP Network, Organization: AddTrust AB, Country: SE"
              },
              "subject": {
                "country_name": "GB",
                "state_or_province_name": "Greater Manchester",
                "locality_name": "Salford",
                "organization_name": "COMODO CA Limited",
                "common_name": "COMODO ECC Certification Authority",
                "distinguished_name": "Common Name: COMODO ECC Certification Authority, Organization: COMODO CA Limited, Locality: Salford, State/Province: Greater Manchester, Country: GB"
              },
              "validity": {
                "not_after": "2020-05-30T10:48:38+00:00",
                "not_before": "2000-05-30T10:48:38+00:00"
              },
              "serial_number": "89484089693757697639156913870987150414",
              "version": "v3",
              "public_key_info": {
                "algorithm": "ec",
                "curve": "secp384r1",
                "public_key": "04:03:47:7b:2f:75:c9:82:15:85:fb:75:e4:91:16:d4:ab:62:99:f5:3e:52:0b:06:ce:41:00:7f:97:e1:0a:24:3c:1d:01:04:ee:3d:d2:8d:09:97:0c:e0:75:e4:fa:fb:77:8a:2a:f5:03:60:4b:36:8b:16:23:16:ad:09:71:f4:4a:f4:28:50:b4:fe:88:1c:6e:3f:6c:2f:2f:09:59:5b:a5:5b:0b:33:99:e2:c3:3d:89:f9:6a:2c:ef:b2:d3:06:e9",
                "key_size": 384,
                "sha256_fingerprint": "e7:ca:91:bb:fb:b1:87:88:05:7b:3a:80:70:44:6e:a5:29:11:60:19:41:02:f7:dc:c3:b9:84:8c:63:cb:9c:d5"
              },
              "extensions": {
                "authority_key_identifier": {
                  "key_identifier": "ad:bd:98:7a:34:b4:26:f7:fa:c4:26:54:ef:03:bd:e0:24:cb:54:1a"
                },
                "key_identifier": "75:71:a7:19:48:19:bc:9d:9d:ea:41:47:df:94:c4:48:77:99:d3:79",
                "key_usage": [
                  "digital_signature",
                  "key_cert_sign",
                  "crl_sign"
                ],
                "basic_constraints": {
                  "ca": true
                },
                "certificate_policies": [
                  {
                    "policy_identifier": "any_policy"
                  }
                ],
                "crl_distribution_points": [
                  {
                    "distribution_point": [
                      "http://crl.trust-provider.com/AddTrustExternalCARoot.crl"
                    ]
                  }
                ],
                "authority_information_access": [
                  {
                    "access_method": "ocsp",
                    "access_location": "http://ocsp.trust-provider.com"
                  }
                ]
              },
              "self_issued": false,
              "self_signed": false
            },
            "spki_subject_fingerprint": "18:87:d2:b5:a9:61:7d:c0:a5:bd:31:be:b1:9b:4c:9f:f8:f9:6a:b4:45:c1:39:e6:52:8d:6d:ff:21:8e:8e:71"
          }
        ]
      }
    }
  }
}
```
