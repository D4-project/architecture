%%%
Title = "D4 Encapsulation Protocol"
abbrev = "D4 Encapsulation Protocol"
category = "info"
docName = "draft-dulaunoy-d4-encapsulation-protocol"
ipr= "trust200902"
area = "Security"
submissiontype = "independent"

[seriesInfo]
name = "Internet-Draft"
value = "draft-01"
stream = "independent"
status = "informational"

[[author]]
initials="A."
surname="Dulaunoy"
fullname="Alexandre Dulaunoy"
abbrev="CIRCL"
organization = "Computer Incident Response Center Luxembourg"
 [author.address]
 email = "alexandre.dulaunoy@circl.lu"
 phone = "+352 247 88444"
 [author.address.postal]
 street = "122, rue Adolphe Fischer"
 city = "Luxembourg"
 code = "L-1521"
 country = "Luxembourg"
%%%

.# Abstract

This document describes the D4 encapsulation protocol, a protocol for encapsulating various types of security data for transmission. The protocol includes a header format, a set of defined data types, and a mechanism for extending the protocol with meta-types.

This document describes the D4 encapsulation protocol, designed to be a lightweight solution for streaming the collection of various data types. Its core design principles are to enable the creation of dedicated sensor networks with a minimal architecture, to support a wide variety of sensor types (software, hardware, or virtual) through a simple software stack, and to provide an all-in-one open-source solution with minimal dependencies. The protocol also supports information sharing capabilities to interconnect different D4 sensor networks and includes a set of default configurations for common monitoring tasks like Passive DNS collection, network flow, packet capture and DDoS backscatter analysis. This document details the protocol's header format, data types, and extension mechanisms.

{mainmatter}

# D4 Encapsulation Protocol

## Introduction

This document specifies the D4 encapsulation protocol. The protocol is designed to be a flexible and extensible way to transport different kinds of security-related data, such as network captures, log files, and sensor telemetry.

##  Conventions and Terminology

The key words "**MUST**", "**MUST NOT**", "**REQUIRED**", "**SHALL**", "**SHALL NOT**",
"**SHOULD**", "**SHOULD NOT**", "**RECOMMENDED**", "**MAY**", and "**OPTIONAL**" in this
document are to be interpreted as described in RFC 2119 [@!RFC2119].

## D4 Packet Header

The D4 packet header is a fixed-size header that precedes the encapsulated data.

| Name          | bit size  | Description                               |
|---------------|-----------|:-----------------------------------------:|
| version       | uint 8    | Version of the header                     |
| type          | uint 8    | Data encapsulated type                    |
| uuid          | uint 128  | Sensor UUID                               |
| timestamp     | uint 64   | Encapsulation time                        |
| hmac          | uint 256  | Authentication header (HMAC-SHA-256-128)  |
| size          | uint 32   | Payload size                              |

## D4 Packet Types

The `type` field in the D4 header indicates the format of the encapsulated data. The following types are defined:

|Type| Description |
|----|:-----------------------------------|
| 0  | Reserved                           |
| 1  | pcap (libpcap 2.4)                 |
| 2  | meta header (JSON)                 |
| 3  | generic log line                   |
| 4  | [dnscap](https://github.com/DNS-OARC/dnscap) output                      |
| 5  | pcapng (diagnostic)                |
| 6  | generic NDJSON or JSON Lines       |
| 7  | [YAF](https://tools.netsa.cert.org/yaf/index.html) (Yet Another Flowmeter)|
| 8  | [passivedns](https://github.com/gamelinux/passivedns) CSV stream |
| 254 | type defined by meta header (type 2) |

A full list of D4 types is available in JSON format at [https://raw.githubusercontent.com/D4-project/architecture/master/format/type.json](https://raw.githubusercontent.com/D4-project/architecture/master/format/type.json).


## Meta Header

The meta header provides a mechanism to extend the D4 protocol with new data types. When a new session is initiated, a packet with `type` 2 (meta header) MUST be sent to the D4 server to describe how to decode subsequent packets. The payload of the meta header is a single JSON object.

### Meta Header Fields

The JSON object in the meta header payload MUST contain a `type` field.

Here is an example of a meta header JSON object:

```json
{
  "type": "ja3-jl",
  "encoding": "utf-8",
  "tags": [
    "tlp:white"
  ],
  "misp:org": "5b642239-4db4-4580-adf4-4ebd950d210f"
}
```

### Meta Types

The following meta-types are defined:

|Type| Description |
|----|:-----------------------------------|
| ja3-jl  | JA3 fingerprinting JL version |
| d4-telemetry | D4 project sensor telemetry |
| fascia       | fascia JSON object |
| maltrail     | [maltrail](https://github.com/stamparm/maltrail) logging |

A full list of D4 meta-types is available in JSON format at [https://raw.githubusercontent.com/D4-project/architecture/master/format/meta-type.json](https://raw.githubusercontent.com/D4-project/architecture/master/format/meta-type.json).

## Security Considerations

The D4 protocol includes an `hmac` field in the header for authentication. Implementers should ensure that strong keys are used and that the HMAC verification is performed correctly to prevent spoofing and data tampering.

## IANA Considerations

This document does not require any IANA actions.

## References

<reference anchor='MISP-P' target='https://github.com/MISP'>
  <front>
   <title>MISP Project - Open Source Threat Intelligence Platform and Open Standards For Threat Information Sharing</title>
   <author initials='' surname='MISP' fullname='MISP Community'></author>
   <date></date>
  </front>
</reference>

{backmatter}
