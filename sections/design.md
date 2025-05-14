# Design {#design}

{: #resource-as-source-of-truth}
{{&protocol}} is predicated on a resource being the most intuitive source for notifying its events. {{&protocol}} treats notifications as a response to a query for an event occurring on the resource. With HTTP allowing representations to provide a potentially unbounded stream of data, the {{&protocol}} Protocol is also able to communicate multiple events on the resource as a stream of notifications.

{: #no-endpoints}
Unlike other notification protocols, {{&protocol}} does not (usually) require additional resources to be specifically dedicated as endpoints for delivering notifications. By giving a resource the ability to send notifications when an event occurs, {{&protocol}} aims to reduce the complexity of both servers and clients implementing notifications, making it easier to develop real-time applications.

## Goals {#goals}

{: #goals-list}
The goals of the {{&protocol}} are:

1. {: #goal--http-only}
to provide notifications only using the Hypertext Transfer Protocol {{HTTP}}, so that the clients fetching resources using HTTP are not required to switch to another protocol for receiving event notifications.

1. {: #goal--unified-source-of-truth}
to provide updates directly from a resource of interest, obviating the need to create another endpoint for event notifications, minimizing round-trips between clients and servers and eliminating the need to co-ordinate responses between a resource and the notification endpoint.

1. {: #goal--unified-request}
to allow clients to fetch representation and notifications in response to a single request, minimizing round-trips between clients and servers and eliminating the need to co-ordinate timing that might have arisen from multiple requests.

1. {: #goal--negotiation}
to allow event notifications to be communicated in any arbitrary format. Implementers shall be able to provide notifications that are potentially more expressive when compared to existing HTTP based messaging protocols such as Server Sent Events {{SSE}}.

1. {: #goal--intermediation}
to specify transparent semantics that allow intermediaries to aid in scaling, reliability, and reducing latency of event notifications as well as proactively update caches.

## Constraints {#constraints}

{: #constraints-list}
To the extent feasible, the {{&protocol}}:

1. {: #constraint--reuse}
adheres to established practices and conventions. In particular, every attempt has been made to reuse existing protocols and specifications. Implementers shall be able to repurpose existing tools and libraries for implementing this specification.

1. {: #constraint--rest}
conforms to the REST Architectural Style {{REST}} and best practices for Building Protocols with HTTP {{RFC9205}}. This specification can, thus, be used to extend the capabilities of any existing HTTP resource to provide notifications. Implementers shall be able to scale notifications along with their data/applications.
<!--
  See my original comment on the Solid/Specification Gitter channel on 24 April 2020
  https://matrix.to/#/!PlIOdBsCTDRSCxsTGA:gitter.im/$VgCcuq2HbpLKJvxIw4witAUOsqcdhC98glgzqVI1WOY
-->

## Scope {#scope}

{: #in-scope}
The {{&protocol}} Protocol specifies:

1. {: #scope--discovery}
A mechanism to discover notification capabilities on a resource.

1. {: #scope--request}
A mechanism to request event notifications from a resource.

1. {: #scope--query-data-model}
A data-model for the notifications query.

1. {: #scope--single-response-semantics}
Semantics for the response that serves single event notification.

1. {: #scope--streaming-response-semantics}
Semantics for the streaming response that serve multiple notifications as well as the representation.


{: #out-of-scope}
The {{&protocol}} Protocol does not specify:

+ {: #no-scope--representations}
Specific representations for request or response. For the purposes of illustration, we shall use:
  + An imaginary `example/event-request` media-type for the request.
  + An imaginary `example/event-response` media-type for the response with a single event notification.
  + `application/http` media-type ({{-HTTP1, Section 10.2}}) as the composite media-type for the response that includes a representation and/or multiple event notifications.

## Limitations {#limitations}

{: #limitation--resource-specificity}
{{&protocol}} only allows notifications to be sent for events on a given resource. To send notifications for events on multiple resources, implementations will need to mint separate resources as notification endpoints. This is no different from APIs built on top of existing messaging protocols (See, for example, {{WS}} and {{WEBSUB}}).

{: #limitation--connection-limits}
Browsers cap the number of persistent HTTP/1.1 connections per host, limiting the suitability of {{&protocol}} for web applications in the browser that require simultaneous notifications from multiple resources on the same host. This limitation is identical to that of other HTTP streaming based protocols, such as Server-Sent Events {{SSE}}. Implementations are strongly encouraged to adopt HTTP/2 (or later). HTTP/1.1 servers might consider setting up a reverse proxy over HTTP/2 (or later) or implement mitigation strategies, such as to maximize the number of concurrent connections and to provide alternate hosts for resources. Implementations might alternatively consider using endpoints to provide notifications for multiple resources as previously described. Clients on a browser requesting notifications over an HTTP/1.1 connection are advised to exercise caution when opening multiple simultaneous persistent connections to any given host.
