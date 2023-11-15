# The Project Architecture Decision Record

- Status: accepted
- Deciders: Arun Poudel (<fr3ak@fr3ak.me>)
- Date: 2023-11-13

## Context and Problem Statement

Some of the key requirements are:

- Stream Data from Mastadon
- Store Data in Big Query
- Provide a gRPC API to fetch/stream
 data

## Assumptions

Various assumptions made when designing options:

- We are a Google Cloud Workshop
- There might be requirement in future to have
multiple data sources (Currently only Mastadon Server)
- There might be requirement in future to have
multiple data sinks (Currently only Big Query and gRPC Service)

## Considered Options

### The Project System Diagram - Option 1

```mermaid
C4Context
    title The Project System Diagram - Option 1

    System_Ext(mastadon, "Mastadon", "A federated social network")
    ComponentDb_Ext(big_query, "Big Query")
    ComponentDb_Ext(google_cloud_sql, "Google Cloud SQL")

    System(mastadon_listener, "Mastadon Listener")
    System(grpc_server, "GRPC Server")

    Rel_D(mastadon, mastadon_listener, "Push")
    Rel_D(mastadon_listener, grpc_server, "Store")
    Rel_D(mastadon_listener, big_query, "Store")

    BiRel(grpc_server, google_cloud_sql, "Reads/Writes")

    Person(api_user, "User", "A user of the API")

    Rel_U(api_user, grpc_server, "Fetch/Subscribe", "GRPC")
```