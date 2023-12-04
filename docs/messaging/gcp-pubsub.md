<!--- Hugo front matter used to generate the website version of this page:
linkTitle: Google Cloud Pub/Sub
--->

# Semantic Conventions for Google Cloud Pub/Sub

**Status**: [Experimental][DocumentStatus]

The Semantic Conventions for [Google Cloud Pub/Sub](https://cloud.google.com/pubsub) extend and override the [Messaging Semantic Conventions](README.md) that describe common messaging operations attributes in addition to the Semantic Conventions described on this page.

`messaging.system` MUST be set to `"gcp_pubsub"`.

## Span attributes

For Google Cloud Pub/Sub, the following additional attributes are defined:
<!-- semconv messaging.gcp_pubsub(full,tag=tech-specific-gcp-pubsub) -->
| Attribute  | Type | Description  | Examples  | Requirement Level |
|---|---|---|---|---|
| [`messaging.gcp_pubsub.message.ordering_key`](../attributes-registry/messaging.md) | string | The ordering key for a given message. If the attribute is not present, the message does not have an ordering key. | `ordering_key` | Conditionally Required: If the message type has an ordering key set. |
<!-- endsemconv -->


## a

```mermaid
flowchart TD;
  subgraph CONSUMER
  direction LR
  A[Ambient]
  R1[Receive m1]
  SM1[Settle m1]
  LM1[Lease m1]
  %% Link 0
  A-- parent -->R1
  %% Link 1
  A-- parent -->SM1
  %% Link 2
  A-- parent -->LM1
  end
  subgraph PRODUCER
  direction LR
  CM1[Create m1]
  PM1[Publish m1]
  end
  %% Link 3
  CM1-. link .-PM1;
  %% Link 4
  CM1-. link .-R1;
  %% Link 5
  CM1-. link .-SM1;
  %% Link 6
  CM1-. link .-LM1;

  %% Style the node and corresponding link
  %% Producer links and nodes
  classDef producer fill:green
  class PM1,CM1 producer
  linkStyle 3 color:green,stroke:green

  %% Consumer links and nodes
  classDef consumer fill:#032a61
  class R1 consumer
  linkStyle 4 color:#032a61,stroke:#032a61

  classDef lease fill:#0560f2
  class LM1 lease
  linkStyle 6 color:#0560f2,stroke:#0560f2

  classDef ack fill:#577eb5
  class SM1 ack
  linkStyle 5 color:#577eb5,stroke:#577eb5

  classDef additional opacity:0.4
  class A additional
```

## b

```mermaid
flowchart TD;
  subgraph CONSUMER
  direction LR
    A[Ambient]
  RM1[Receive m1]
  SM1[Settle m1]
  LM1[Lease m1]
   RM2[Receive m2]
  SM2[Settle m2]
  LM2[Lease m2]  
  %% Link 0 
   A-- parent -->RM1
  %% Link 1
   A-- parent -->SM1
   %% Link 2 
   A-- parent -->RM2
   %% Link 3 
  A-- parent -->SM2
     %% Link 4
   A-- parent -->LM1
   %% Link 5
  A-- parent -->LM2

  end
  subgraph PRODUCER
  direction LR
  CM1[Create m1]
  CM2[Create m2]
  P[Publish]
  end
   %% Link 6 
CM1-. link .-P;
   %% Link 7 
  CM2-. link .-P;
    %% Link 8 
 CM1-. link .-RM1;
    %% Link 9
 CM2-. link .-RM2;
       %% Link 10
 CM1-. link .-LM1
    %% Link 11
 CM2-. link .-LM2
     %% Link 12
 CM1-. link .-SM1;
   %% Link 13
  CM2-. link .-SM2;

  %% Style the node and corresponding link
  %% Producer links and nodes
  classDef producer fill:green
  class P,CM1,CM2 producer
  linkStyle 7,6 color:green,stroke:green

  %% Consumer links and nodes
  classDef consumer fill:#032a61
  class RM1,RM2 consumer
  linkStyle 8,9 color:#032a61,stroke:#032a61

  classDef lease fill:#0560f2
  class LM1,LM2 lease
  linkStyle 10,11 color:#0560f2,stroke:#0560f2

  classDef ack fill:#577eb5
  class SM1,SM2 ack
  linkStyle 12,13 color:#577eb5,stroke:#577eb5

  classDef additional opacity:0.4
  class A additional
```



## stream
```mermaid
flowchart TD;
  subgraph CONSUMER
  direction LR
  A[Ambient]-- parent -->RM1[Receive m1 m2]
  A-- parent -->SM1[Settle m1 m2]
  A-- parent -->L[Lease m1 m2]
  end
  subgraph PRODUCER
  direction LR
  CM1[Create m1]
  CM2[Create m2]
  P[Publish]
  end
  CM1-. link .-P;
  CM2-. link .-P;
  CM1-. link .-RM1;
  CM2-. link .-RM1;
  CM1-. link .-SM1;
  CM2-. link .-SM1;
  CM1-. link .-L;
  CM2-. link .-L;

  classDef ack fill:#5e04d4
  class SM1 ack
  classDef consumer fill:#0560f2
  class RM1 consumer
  classDef producer fill:green
  class P,CM1,CM2 producer
  classDef lease fill:#b33ba1
  class L lease
  classDef additional opacity:0.4
  class A additional
  linkStyle 3,4 color:green,stroke:green 
  linkStyle 5,6 color:#0560f2,stroke:#0560f2
  linkStyle 7,8 color:#5e04d4,stroke:#5e04d4
  linkStyle 9,10 color:#b33ba1,stroke:#b33ba1
```

[DocumentStatus]: https://github.com/open-telemetry/opentelemetry-specification/tree/v1.26.0/specification/document-status.md

```mermaid
flowchart LR;
  subgraph PRODUCER
  direction TB
  CA[Span Create A]
  CB[Span Create B]
  P[Span Publish]
  end
  subgraph CONSUMER1
  direction TB
  D1[Span Receive A]
  end
  subgraph CONSUMER2
  direction TB
  D2[Span Receive B]
  end
  CA-. link .-D1;
  CB-. link .-D2;
  CA-. link .-P;
  CB-. link .-P;

  classDef normal fill:green
  class P,CA,CB,D1,D2 normal
  linkStyle 0,1,2,3 color:green,stroke:green
```



```mermaid
flowchart TD;
  subgraph CONSUMER2
  direction LR
  RB[Receive B]
  end
  subgraph CONSUMER1
  direction LR
  RA[Receive A]
  end
  subgraph PRODUCER
  direction LR
  CA[Create A]
  CB[Create B]
  P[Publish]
  end
  %% Link 0
  CA-. link .-P;  
  %% Link 1
  CB-. link .-P;
  %% Link 2
  CA-. link .-RA;
  %% Link 3
  CB-. link .-RB;

  %% Style the nodes and corresponding links
  %% producer links and nodes
  classDef producer fill:green
  class P,CA,CB producer
  linkStyle 0,1 color:green,stroke:green

  %% consumer1 links and nodes with light blue
  classDef consumer1 fill:#9eecff,color:black
  class RA consumer1
  linkStyle 2 color:#9eecff,stroke:#9eecff

  %% consumer2 links and nodes with dark blue
  classDef consumer2 fill:#032a61
  class RB consumer2
  linkStyle 3 color:#032a61,stroke:#032a61
```


