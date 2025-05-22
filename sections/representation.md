# Representation {#representation}

{{&protocol}} allows a user agent to simultaneously request for a representation along with event-notifications. This not only saves on an extra round trip, but relieves a user agent from the burden of managing potential race conditions between the representation and event-notifications.

## Request {#representation-request}

{: #representation-request-procedure}
To request a representation of the resource using {{&protocol}}, a client MUST use the subscription data model ({{data-model}}) with the +state+ property in an appropriate media-type when issuing a request using the `QUERY` method ({{HTTP-QUERY, Section 3}}).

{: #representation-request-state-conneg}
The +state+ property MAY be used to specify any header field in the body of the subscription query to negotiate the representation.

~~~ http-message
{::include examples/stream/state-request.http}
~~~
{: sourcecode-name="representation-request-example.http" #representation-request-example title="Representation and Notifications Request"}

## Response {#representation-response}

{: #representation-response-body}
A server able to provide a stream with a representation and event-notifications transmits the representation immediately following the response header ({{stream-response-header}}). Otherwise, the response is the same as that described in {{stream-response}}.

{: #representation-response-encapsulation}
Again, we shall use the `application/http` media-type ({{-HTTP1, Section 10.2}}) for the purpose of illustration.

~~~ http-message
{::include examples/stream/response-headers.http}

{::include examples/stream/representation.http}
~~~
{: sourcecode-name="representation-response-example.http" #representation-response-example title="Representation Response"}

{: #representation-response-non-standard}
While this is default behaviour, there is no requirement that a representation is the first message or is sent only once. In such cases, the encapsulated message needs to indicate if it is a representation and not an event-notification. Such a mechanism is not defined in this specification.

{: #representation-response-notifications}
Notifications are transmitted just as described in {{stream-response}}. See {{example}} for a complete example of a response with representation and notifications.
