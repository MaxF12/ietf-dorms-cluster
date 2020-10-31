---
title: Discovery Of Restconf Metadata for Source-specific multicast
abbrev: DORMS
docname: draft-ietf-mboned-dorms-01
date: 2020-10-30
category: std

ipr: trust200902
area: Ops
workgroup: Mboned
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: J. Holland
    name: Jake Holland
    org: Akamai Technologies, Inc.
    street: 150 Broadway
    city: Cambridge, MA 02144
    country: United States of America
    email: jakeholland.net@gmail.com

normative:
  RFC2119:
  RFC2782:
  RFC3596:
  RFC6991:
  RFC7950:
  RFC8040:
  RFC8174:
  RFC8294:
  RFC8340:
  RFC8341:
  whatwg-fetch:
    target: https://fetch.spec.whatwg.org/
    title: "WHATWG Fetch Living Standard"
    date: October 2020

informative:
  RFC1034:
  RFC1035:
  RFC2845:
  RFC2931:
  RFC3040:
  RFC3376:
  RFC3688:
  RFC3810:
  RFC4604:
  RFC4607:
  RFC6020:
  RFC6335:
  RFC6415:
  RFC7858:
  RFC8446:
  RFC8484:
  I-D.draft-ietf-mboned-cbacc-00:
  I-D.draft-ietf-mboned-ambi-00:

--- abstract

This document defines DORMS (Discovery Of Restconf Metadata for Source-specific multicast), a method to discover and retrieve extensible metadata about source-specific multicast channels using RESTCONF.
The reverse IP DNS zone for a multicast sender's IP address is configured to use SRV resource records to advertise the hostname of a RESTCONF server that publishes metadata according to a new YANG module with support for extensions.
A new service name and the new YANG module are defined.

--- middle

# Introduction

This document defines DORMS (Discovery Of Restconf Metadata for Source-specific multicast).

A DORMS service is a RESTCONF {{RFC8040}} service that provides read access to data in the "ietf-dorms" YANG {{RFC7950}} model defined in {{model}}.
This model, along with optional extensions defined in other documents, provide an extensible set of information about multicast data streams.
A review of some example use cases that can be enabled by this kind of metadata is given in {{motivation}}.

This document defines the "dorms" service name for use with the SRV DNS Resource Record (RR) type {{RFC2782}}.
A sender using a DORMS service to publish metadata SHOULD configure at least one SRV RR for the "\_dorms.\_tcp" subdomain in the reverse IP DNS zone for the source IP used by some active multicast traffic.
The domain name in one of these SRV records provides a hostname corresponding to a DORMS server that can provide metadata for the sender's source-specific multicast traffic.
Publishing such a RR enables DORMS clients to discover and query a DORMS server as described in {{disco}}.

## Background

The reader is assumed to be familiar with the basic DNS concepts described in {{RFC1034}}, {{RFC1035}}, and the subsequent documents that update them, as well as the use of the SRV Resource Record type as described in {{RFC2782}}.

The reader is also assumed to be familiar with the concepts and terminology regarding source-specific multicast as described in {{RFC4607}} and the use of IGMPv3 {{RFC3376}} and MLDv2 {{RFC3810}} for group management of source-specific multicast channels, as described in {{RFC4604}}.

The reader is also assumed to be familiar with the concepts and terminology for RESTCONF {{RFC8040}} and YANG {{RFC7950}}.

## Terminology

   Term | Definition
   ----:|:----------
  (S,G) | A source-specific multicast channel, as described in {{RFC4607}}. A pair of IP addresses with a source host IP and destination group IP.
 DORMS client | An application or system that can communicate with DORMS servers to fetch metadata about (S,G)s.
 DORMS server | A RESTCONF server that implements the ietf-dorms YANG model defined in this document.
     RR | A DNS Resource Record, as described in {{RFC1034}}
 RRType | A DNS Resource Record Type, as described in {{RFC1034}}
    SSM | Source-specific multicast, as described in {{RFC4607}}

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in {{RFC2119}} and {{RFC8174}} when, and only when, they appear in all capitals, as shown here.

## Motivation {#motivation}

DORMS provides a framework that can be extended to publish supplemental information about multicas traffic in a globally discoverable manner.
This is useful so that entities engaged in delivery or processing of the traffic that are not affiliated with the sender of the traffic and who may not otherwise have any means to discover information about the traffic, such as forwarding ISPs or operators of firewalls providing security guarantees to their users, can discover the information they may need in order to process the traffic according to their requirements.

### Use cases

For example, a network that is capable of forwarding multicast traffic may need to take provisioning actions or make admission control decisions at ingress points based on the expected bitrate of the traffic in order to prevent oversubscription of the network.

Other use cases may include metadata that can be used to authenticate the multicast traffic, metadata that describes the contents of the traffic, metadata that makes assertions about the legal status of the traffic within specific contexts, or metadata that describes the protocols or applications that can be used to consume the traffic.
Extensions to DORMS to support these or other kinds of metadata can be defined by later specifications.

Detailing the specific of the possible extensions is out of scope for this document except to note that a range of possible use cases are expected and they may be supported by a variety of different future extensions.

### Channel Selection

In general, a DORMS client might learn of an (S,G) by any means.
Therefore, describing the full set of possible methods a DORMS client might use to discover a set of (S,G)s for which it wants metadata is out of scope for this document.

But to give a few examples, a multicast receiver application that is a DORMS client might learn about an (S,G) by getting signals from inside the application logic, such as a selection made by a user, or a scheduled API call that reacts to updates in a library provided by a service operator.

As another example, an on-path router that’s a DORMS client might instead learn about an (S,G) by receiving a PIM message or an IGMP or MLD membership report indicating a downstream client has tried to subscribe to an (S,G).
Such a router might use information learned from the DORMS metadata to make an access control decision about whether to propagate the join futher upstream in the network.

Other approaches for learning an (S,G) could be driven by monitoring a route reflector to discover channels that are being actively forwarded, for a purpose such as monitoring network health.

## Notes for Contributors and Reviewers

Note to RFC Editor: Please remove this section and its subsections before publication.

This section is to provide references to make it easier to review the development and discussion on the draft so far.

### Venues for Contribution and Discussion {#venue}

This document is in the Github repository at:

https://github.com/GrumpyOldTroll/ietf-dorms-cluster

Readers are welcome to open issues and send pull requests for this document. 

Please note that contributions may be merged and substantially edited, and as a reminder, please carefully consider the Note Well before contributing: https://datatracker.ietf.org/submit/note-well/

Substantial discussion of this document should take place on the MBONED working group mailing list (mboned@ietf.org).

 * Join: https://www.ietf.org/mailman/listinfo/mboned
 * Search: https://mailarchive.ietf.org/arch/browse/mboned/

### Non-obvious doc choices

Log of odd things that need to be the way they are because of some reason that the author or reviewers may want to know later.

 * building the draft without this line produces a warning about no reference to {{RFC6991}} or {{RFC8294}}, but these are imported in the yang model. RFC 8407 requires the normative reference to 8294 (there's an exception for 6991 but I'm not sure why and it doesn't seem forbidden).

# Discovery and Metdata Retrieval {#disco}

A client that needs metadata about an (S,G) MAY attempt to discover metadata for the (S,G) using the mechanisms defined here, and MAY use the metadata received to manage the forwarding or processing of the packets in the channel.

## DNS Bootstrap {#dns-boot}

The DNS Bootstrap step is how a client discovers an appropriate RESTCONF server, given the source address of an (S,G).
Use of the DNS Bootstrap is OPTIONAL for clients with an alternate method of obtaining a hostname of a trusted DORMS server with information about the target (S,G).

This mechanism only works for source-specific multicast (SSM) channels.
The source address of the (S,G) is reversed and used as an index into one of the reverse mapping trees (in-addr.arpa for IPv4, as described in Section 3.5 of {{RFC1035}}, or ip6.arpa for IPv6, as described in Section 2.5 of {{RFC3596}}).

When a DORMS client needs metadata for an (S,G), for example when handling a new join for that (S,G) and looking up the authentication methods that are available, a receiver or middlebox can issue a DNS query for a SRV RR using the "dorms" service name with the domain from the reverse mapping tree, combining them as described in {{RFC2782}}.

For example, while handling a join for (203.0.113.15, 232.1.1.1), a receiver would perform a DNS query for the SRV RRType for the domain:

~~~
     _dorms._tcp.15.113.0.203.in-addr.arpa.
~~~

The DNS response for this domain might return a record such as:

~~~
     SRV 0 1 443 dorms-restconf.example.com.
~~~

This response informs the receiver that a DORMS server should be reachable at dorms-restconf.example.com on port 443, and should contain metadata about multicast traffic from the given source IP.
Multiple SRV records are handled as described by {{RFC2782}}.

A sender providing DORMS discovery SHOULD publish at least one SRV record in the reverse DNS zone for each source address of the multicast channels it is sending in order to advertise the hostname of the DORMS server to DORMS clients.
The DORMS servers advertised SHOULD be configured with metadata for all the groups sent from the same source IP address that have metadata published with DORMS.

## Ignore List {#ignore}

If a DORMS client reaches a DORMS server but determines through examination of responses from that DORMS server that it may not understand or be able to use the responses of the server (for example due to an issue like a version mismatch or modules that are missing but are required for the DORMS client's purposes), the client MAY add this server to an ignore list and reject servers in its ignore list during future discovery attempts.

A client using the DNS Bootstrap discovery method in {{dns-boot}} would treat servers in its ignore list as unreachable for the purposes of processing the SRV RR as described in {{RFC2782}}.
(For example, a client might end up selecting a server with a less-preferred priority than servers in its ignore list, even if an HTTPS connection could have been formed successfully with some of those servers.)

If an ignore list is maintained, entries SHOULD time out and allow for re-checking after either the cache expiration time from the response that caused the server to be added to the ignore list, or for a configurable hold-down time that has a default value no shorter than an hour and no longer than 3 days.

## RESTCONF Bootstrap

Once a DORMS host has been chosen (whether via an SRV RR from a DNS response or via some other method), RESTCONF provides all the information necessary to determine the versions and url paths for metadata from the server.
A walkthrough is provided here for a sequence of example requests and responses from a receiver connecting to a new DORMS server.

### Root Resource Discovery

As described in Section 3.1 of {{RFC8040}} and {{RFC6415}}, the RESTCONF server provides the link to the RESTCONF api entry point via the "/.well-known/host-meta" or "/.well-known/host-meta.json" resource.

Example:

The receiver might send:

~~~
     GET /.well-known/host-meta.json HTTP/1.1
     Host: dorms-restconf.example.com
     Accept: application/json
~~~

The server might respond as follows:

~~~
      HTTP/1.1 200 OK
      Date: Tue, 27 Aug 2019 20:56:00 GMT
      Server: example-server
      Cache-Control: no-cache
      Content-Type: application/json

      {
        "links":[
          {
            "rel":"restconf",
            "href":"/top/restconf"
          }
        ]
      }
~~~

### Yang Library Version

As described in Section 3.3.3 of {{RFC8040}}, the yang-library-version leaf is required by RESTCONF, and can be used to determine the schema of the ietf-yang-library module:

Example:

The receiver might send:

~~~
      GET /top/restconf/yang-library-version HTTP/1.1
      Host: dorms-restconf.example.com
      Accept: application/yang-data+json
~~~

The server might respond as follows:

~~~
      HTTP/1.1 200 OK
      Date: Tue, 27 Aug 2019 20:56:01 GMT
      Server: example-server
      Cache-Control: no-cache
      Content-Type: application/yang-data+json

      {
          "ietf-restconf:yang-library-version": "2016-06-21"
      }
~~~

If a DORMS client determines through examination of the yang-library-version that it may not understand the responses of the server due to a version mismatch, the server qualifies as a candidate for adding to an ignore list as described in {{ignore}}.

### Yang Library Contents

After checking that the version of the yang-library module will be understood by the receiver, the client can check that the desired metadata modules are available on the DORMS server by fetching the module-state resource from the ietf-yang-library module.

Example:

The receiver might send:

~~~
      GET /top/restconf/data/ietf-yang-library:modules-state/\
          module=ietf-dorms,2016-08-15
      Host: dorms-restconf.example.com
      Accept: application/yang-data+json
~~~

The server might respond as follows:

~~~
    HTTP/1.1 200 OK
    Date: Tue, 27 Aug 2019 20:56:02 GMT
    Server: example-server
    Cache-Control: no-cache
    Content-Type: application/yang-data+json

    {
      "ietf-yang-library:module": [
        {
          "conformance-type": "implement",
          "name": "ietf-dorms",
          "namespace": "urn:ietf:params:xml:ns:yang:ietf-dorms",
          "revision": "2019-08-25",
          "schema":
              "https://example.com/yang/ietf-dorms@2019-08-25.yang"
        }
      ]
    }
~~~

Other modules required or desired by the client also can be checked in a similar way, or the full set of available modules can be retrieved by not providing a key for the "module" list.
If a DORMS client that requires the presence of certain modules to perform its function discovers the required modules are not present on a server, that server qualifies for inclusion in an ignore list according to {{ignore}}.

### Metadata Retrieval

Once the expected DORMS version is confirmed, the client can retrieve the metadata specific to the desired (S,G).

Example:

The receiver might send:

~~~
      GET /top/restconf/data/ietf-dorms:metadata/\
          sender=203.0.113.15/group=232.1.1.1
      Host: dorms-restconf.example.com
      Accept: application/yang-data+json
~~~

The server might respond as follows:

~~~
      HTTP/1.1 200 OK
      Date: Tue, 27 Aug 2019 20:56:02 GMT
      Server: example-server
      Cache-Control: no-cache
      Content-Type: application/yang-data+json

      {
        "ietf-dorms:group": [
          {
            "group-address":"232.1.1.1",
            "udp-stream":[
              {
                "port":"5001"
              }
            ]
          }
        ]
      }
~~~

Note that when other modules are installed on the DORMS server that extend the ietf-dorms module, other fields MAY appear inside the response.
This is the primary mechanism for providing extensible metadata for an (S,G), so clients SHOULD ignore fields they do not understand.

As mentioned in {{scoping}}, most clients SHOULD use data resource identifiers in the request URI as in the above example, in order to retrieve metadata for only the targeted (S,G)s.

### Cross Origin Resource Sharing (CORS) {#cors-considerations}

It is RECOMMENDED that DORMS servers use the Access-Control-Allow-Origin header field, as specified by {{whatwg-fetch}}, and that they respond appropriately to Preflight requests.

The use of '\*' for allowed origins is NOT RECOMMENDED for DORMS servers.
A review of some of the potential consequences of unrestricted CORS access is given in {{security-cors}}.

# Scalability Considerations

## Provisioning

In contrast to many common RESTCONF deployments that are intended to provide configuration management for a service to a narrow set of authenticated administrators, DORMS servers often provide read-only metadata for public access or for a very large set of end receivers, since it provides metadata in support of multicast data streams and multicast can scale to very large audiences.

Operators are advised to provision the DORMS service in a way that will scale appropriately to the size of the expected audience.
Specific advice on such scaling is out of scope for this document, but some of the mechanisms outlined in {{RFC3040}} or other online resources might be useful, depending on the expected number of receivers.

## Data Scoping  {#scoping}

In the absence of contextual information, clients SHOULD issue narrowed requests for DORMS resources by following the format from Section 3.5.3 of {{RFC8040}} to encode data resource identifiers in the request URI.
This avoids downloading excessive data, since the DORMS server may provide metadata for many (S,G)s, possibly from many different senders.

However, clients MAY use heuristics or out of band information about the service to issue requests for (S,G) metadata narrowed only by the source-address, or not narrowed at all.
Depending on the request patterns and the contents of the data store, this may result in fewer round trips or less overhead, and can therefore be helpful behavior for scaling purposes in some scenarios.
Servers MAY restrict or throttle client access based on the client certificate presented (if any), or based on heuristics that take note of client request patterns.

A complete description of the heuristics for clients and servers to meet their scalability goals is out of scope for this document.

# YANG Model {#model}

The primary purpose of the YANG model defined here is to serve as a scaffold for the more useful metadata that will extend it.
Example specified use cases include providing authentication information {{I-D.draft-ietf-mboned-ambi-00}} and bit-rate information {{I-D.draft-ietf-mboned-cbacc-00}} for use by receivers and middle boxes, but more use cases are anticipated.

## Yang Tree

The tree diagram below follows the notation defined in {{RFC8340}}.

~~~~~~~~~
YANG-TREE ietf-dorms.yang
~~~~~~~~~
{: title="DORMS Tree Diagram" }

## Yang Module

~~~~~~~~~
YANG-MODULE ietf-dorms.yang
~~~~~~~~~

# Privacy Considerations

## Linking Content to Traffic Streams

In the typical case, the mechanisms defined in this document provide a standardized way to discover information that is already available in other ways.

However, depending on the metadata provided by the server, observers may be able to more easily associate traffic from an (S,G) with the content contained within the (S,G).
At the subscriber edge of a multicast-capable network, where the network operator has the capability to localize an IGMP {{RFC3376}} or MLD {{RFC3810}} channel subscription to a specific user or location, for example by MAC address or source IP address, the structured publishing of metadata may make it easier to automate collection of data about the content a receiver is consuming.

## Linking Multicast Subscribers to Unicast Connections

Subscription to a multicast channel generally only exposes the IGMP or MLD membership report to others on the same LAN, and as the membership propagates through a multicast-capable network, it ordinarily gets aggregated with other end users.

However, a RESTCONF connection is a unicast connection, and exposes a different set of information to the operator of the RESTCONF server, including IP address and timing about the requests made.
Where DORMS access becomes required to succeed a multicast join, as expected in a browser deployment, this can expose new information about end users relative to services based solely on multicast streams.

In some deployments it may be possible to use a proxy that aggregates many end users when the aggregate privacy characteristics are needed by end users.

# IANA Considerations

## The YANG Module Names Registry

This document adds one YANG module to the "YANG Module Names" registry maintained at \<https://www.iana.org/assignments/yang-parameters\>.
The following registrations are made, per the format in Section 14 of {{RFC6020}}:

~~~
      name:      ietf-dorms
      namespace: urn:ietf:params:xml:ns:yang:ietf-dorms
      prefix:    dorms
      reference: I-D.draft-ietf-mboned-dorms
~~~

## The XML Registry

This document adds the following registration to the "ns" subregistry of the "IETF XML Registry" defined in {{RFC3688}}, referencing this document.

~~~
       URI: urn:ietf:params:xml:ns:yang:ietf-dorms
       Registrant Contact: The IESG.
       XML: N/A, the requested URI is an XML namespace.
~~~

## The Service Name and Transport Protocol Port Number Registry

This document adds one service name to the "Service Name and Transport Protocol Port Number Registry" maintained at \<https://www.iana.org/assignments/service-names-port-numbers\>.
The following registrations are made, per the format in Section 8.1.1 of {{RFC6335}}:

~~~
     Service Name:            dorms
     Transport Protocol(s):   TCP, UDP
     Assignee:                IESG <iesg@ietf.org>
     Contact:                 IETF Chair <chair@ietf.org>
     Description:             The DORMS service (RESTCONF that
                              includes ietf-dorms YANG model)
     Reference:               I-D.draft-ietf-mboned-dorms
     Port Number:             N/A
     Service Code:            N/A
     Known Unauthorized Uses: N/A
     Assignment Notes:        N/A
~~~

# Security Considerations {#security}

## YANG Model Considerations

The YANG module specified in this document defines a schema for data that is designed to be accessed via RESTCONF {{RFC8040}}.  The lowest RESTCONF layer is HTTPS, and the mandatory-to-implement secure transport is TLS {{RFC8446}}.

The Network Configuration Access Control Model (NACM) {{RFC8341}} provides the means to restrict access for particular NETCONF or RESTCONF users to a preconfigured subset of all available NETCONF or RESTCONF protocol operations and content.  DORMS servers MAY use NACM to control access to data nodes.

No data nodes defined in this YANG module are writable, creatable, or deletable.  This YANG module is intended for publication of read-only data according to a well-defined schema.

## Exposure of Metadata

Although some DORMS servers MAY restrict access based on client identity, as described in Section 2.5 of {{RFC8040}}, many DORMS servers will use the ietf-dorms YANG model to publish information without restriction, and even DORMS servers requiring client authentication will inherently, because of the purpose of DORMS, be providing the DORMS metadata to potentially many receivers.

Accordingly, future YANG modules that augment data paths under "ietf-dorms:metadata" MUST NOT include any sensitive data unsuitable for public dissemination in those data paths.

Because of the possibility that scalable read-only access might be necessary to fulfill the scalability goals for a DORMS server, data under these paths MAY be cached or replicated by numerous external entities, so owners of such data SHOULD NOT assume such data can be kept secret when provided by DORMS servers anywhere under the "ietf-dorms:metadata" path even if access controls are used with authenticated clients unless  additional operational procedures and restrictions are defined and implemented that can effectively control the dissemination of the secret data.
DORMS alone does not provide any such mechanisms, and users of DORMS can be expected not to be following any such mechanisms in the absence of additional assurances.

## Secure Communications

The provisions of Section 2 of {{RFC8040}} provide secure communication requirements that are already required of DORMS servers, since they are RESTCONF servers.
All RESTCONF requirements and security considerations remain in force for DORMS servers.

It is intended that security related metadata about the SSM channels such as public keys for use with cryptographic algorithms may be delivered over the RESTCONF connection, and that information available from this connection can be used as a trust anchor.
The secure transport provided by these minimum requirements are relied upon to provide authenticated delivery of these trust anchors, once a connection with a trusted DORMS server has been established.

## Record-Spoofing

 When using the DNS Boostrap method of discovery described in {{dns-boot}}, the SRV resource record contains information that SHOULD be communicated to the DORMS client without being modified.  The method used to ensure the result was unmodified is up to the client.

 There must be a trust relationship between the end consumer of this resource record and the DNS server.
This relationship may be end-to-end DNSSEC validation or a secure connection to a trusted DNS server that provides end-to-end safety to prevent record-spoofing of the response from the trusted server.
The connection to the trusted server can use any secure channel, such as with a TSIG {{RFC2845}} or SIG(0) {{RFC2931}} channel, a secure local channel on the host, DNS over TLS {{RFC7858}}, DNS over HTTPS {{RFC8484}}, or some other mechanism that provides authentication of the RR.

If a DORMS client accepts a maliciously crafted SRV record, the client could connect to a server controlled by the attacker, and use metadata provided by them.  The consequences of trusting maliciously crafted metadata could range from attacks against the DORMS client's parser of the metadata (via malicious constructions of the formatting of the data) to arbitrary disruption of the decisions the DORMS client makes as a result of processing validly constructed metadata.

Clients MAY use other secure methods to explicitly associate an (S,G) with a set of DORMS server hostnames, such as a configured mapping or an alternative trusted lookup service.

## CORS considerations {#security-cors}

As described in {{cors-considerations}}, it's RECOMMENDED that DORMS servers provide appropriate restrictions to ensure only authorized web pages access metadata for their (S,G)s from the widely deployed base of secure browsers that use the CORS protocol according to {{whatwg-fetch}}.

Providing '\*' for the allowed origins exposes the DORMS-based metadata to access by scripts in all web pages, which opens the possibility of certain kinds of attacks against networks where browsers have support for joining multicast (S,G)s.

If the authentication for an (S,G) relies on DORMS-based metadata (for example, as defined in {{I-D.draft-ietf-mboned-ambi-00}}), an unauthorized web page that tries to join an (S,G) not permitted by the CORS headers for the DORMS server will be prevented from subscribing to the channels.

If an unauthorized site is not prevented from subscribing, code on the site (for example a malicious advertisement) could request subscriptions from many different (S,G)s, overflowing limits on the joining of (S,G)s and disrupting the delivery of multicast traffic for legitimate use.

Further, if the malicious script can be distributed to many different users within the same receiving network, the script could coordinate an attack against the network as a whole by joining disjoint sets of (S,G)s from different users within the receiving network.
The distributed subscription requests across the receiving network could overflow limits for the receiving network as a whole, essentially causing the websites displaying the ad to participate in an overjoining attack (see Appendix A of {{I-D.draft-ietf-mboned-cbacc-00}}).

Even if network safety mechanisms protect the network from the worst effects of oversubscription, the population counts for the multicast subscriptions could be disrupted by this kind of attack, and therefore push out legitimately requested traffic that's being consumed by real users.
For a legitimately popular event, this could cause a widespread disruption to the service if it's successfully pushed out.

A denial of service attack of this sort would be thwarted by restricting the access to (S,G)s to authorized websites through the use of properly restricted CORS headers.

# Acknowledgements

Thanks to Christian Worm Mortensen, Dino Farinacci, and Lenny Guiliano for their very helpful comments and reviews.

