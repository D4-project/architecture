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

Sample meta type JSON (type 2). If a new session is open, before sending D4 packet type 254, a type 2 packet MUST be sent
to describe to the D4 server how to decode packets. A meta header payload contains a single JSON object which describes
the next packet to be decoded as type 254 in the stream. The JSON object MUST at least contain a `type` field.

~~~~json
{
  "type": "ja3-jl",
  "encoding": "utf-8",
  "tags": [
    "tlp:white"
  ],
  "misp:org": "5b642239-4db4-4580-adf4-4ebd950d210f"
}
~~~~

|Type| Description |
|----|:-----------------------------------|
| ja3-jl  | JA3 fingerprinting JL version |
| d4-telemetry | D4 project sensor telemetry |
| fascia       | fascia JSON object |
