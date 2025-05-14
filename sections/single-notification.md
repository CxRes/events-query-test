# Single Notification {#single-notification}

The simplest event query is to ask for the next event on a resource. This, in effect, overloads a resource with long-polling capability.

## Request {#single-notification-request}

A client can send an empty query to be notified of a single event. A server MUST interpret an empty query with an appropriate media-type made using the `QUERY` method ({{HTTP-QUERY, Section 3}}) as a request for a single event notification.

A client can further negotiate the form of the event notification using header fields.

~~~ http-message
{::include examples/single-notification/request.http}
~~~
{: sourcecode-name="single-notification-request-example.http" #single-notification-request-example title="Single Event Notification Request"}

<!--
## Timeout {#timeout}

Sometimes, a server needs to close the connection before an event occurs. To allow servers to signal the intentional closure of the stream, we define the `555 (Connection Timeout)` status code.

If the server needs to close the connection, such as, on account of exceeding the time limit specified in the =duration= property of the =Events= header field, it MUST send a `555 (Connection Timeout)` status code in the response.

// Trailer fields to close connection
-->

## Response {#single-notification-response}

A server MUST NOT send a !notification until after the event has been completed. When providing a single !notification, the server MUST close the connection immediately after transmitting the event notification.

~~~ http-message
{::include examples/single-notification/response.http}
~~~
{: sourcecode-name="single-notification-response-example.http" #single-notification-response-example title="Single Event Notification Response"}

{:aside}
> **Implementation Guidance**
>
> Implementations are advised against sending event notifications for long-lived resources over HTTP. A resource might be considered long-lived, if a server determines that the resource is unlikely to change in the duration of the notification response. In such instances, resource servers are strongly advised to respond with the `Cache-Control` ({{HTTP-CACHING, Section 5}}) header field and the `max-age` parameter ({{HTTP-CACHING, Section 5.2.2.1}}) set in it.
