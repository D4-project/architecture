# D4 encapsulation protocol version 1 (DRAFT)

![Overview of the D4 encapsulation protocol](https://raw.githubusercontent.com/D4-project/architecture/master/docs/diagram/d4-protocol-encapsulation.png)

## Headers

| Name          | bit size  |                               Description                              |
|---------------|-----------|:----------------------------------------------------------------------:|
| version       | uint 8    | Version of the header                                                  |
| type          | uint 8    | Data encapsulated type                                                 |
| uuid          | uint 128  | Sensor UUID                                                            |
| timestamp     | uint 64   | Encapsulation time                                                     |
| hmac          | uint 256  | Header authentication (HMAC-SHA-256-128)                               |
| size          | uint 32   | Payload size                                                           |

## Type

|Type| Description |
|----|:-----------------------------------|
| 0  | Reserved                           |
| 1  | pcap (libpcap 2.4)                 |
| 2  | meta header (JSON)                 |
| 3  | generic log line                   |
| 4  | dnscap output                      |
| 5  | pcapng                             |
| 6  | generic NDJSON or JSON Lines       |
| 7  | generic YAF (Yet Another Flowmeter)|

