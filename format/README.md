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

## Types

The type is the list of format encapsulated within the D4 protocol.

|Type| Description |
|----|:-----------------------------------|
| 0  | Reserved                           |
| 1  | pcap (libpcap 2.4)                 |
| 2  | meta header (JSON)                 |
| 3  | generic log line                   |
| 4  | [dnscap](https://github.com/DNS-OARC/dnscap) output                      |
| 5  | pcapng (diagnostic)                |
| 6  | generic NDJSON or JSON Lines       |
| 7  | generic [YAF](https://tools.netsa.cert.org/yaf/index.html) (Yet Another Flowmeter)|
| 254 | type defined by meta header (type 2) |

The D4 type list is [available in JSON format](https://raw.githubusercontent.com/D4-project/architecture/master/format/type.json).

## Meta types (via meta header)

Sample meta type JSON

~~~~json
{
  "type": "1337",
  "encoding": "utf-8",
  "tags": [
    "tlp:white"
  ],
  "misp:org": "5b642239-4db4-4580-adf4-4ebd950d210f"
}
~~~~

|Type| Description |
|----|:-----------------------------------|
| 0  | Reserved                           |
| 1  | pcap (libpcap 2.4)                 |
| 2  | Reserved                           |
| 3  | generic log line                   |
| 4  | [dnscap](https://github.com/DNS-OARC/dnscap) output                      |
| 5  | pcapng (diagnostic)                |
| 6  | generic NDJSON or JSON Lines       |
| 7  | generic [YAF](https://tools.netsa.cert.org/yaf/index.html) (Yet Another Flowmeter)|
| 254 | Reserved |
| 1337 |  ja3-jl                          |
