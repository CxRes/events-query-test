# Single Notification {#single-notification}

{: #single-notification-description}
The simplest event query is to ask for a notification for the next event(s) on a resource. This, in effect, adds long-polling capability ({{RFC6202, Section 2}}) to a resource.

## Request {#single-notification-request}

{: #single-notification-request-content}
A client can send an empty query to be notified of a single event. A server MUST interpret a request with an empty query in an appropriate media-type made using the `QUERY` method ({{HTTP-QUERY, Section 3}}) as a subscription request for a single event-notification.

{: #single-notification-request-conneg}
A client can, as usual, negotiate the form of the event-notification using header fields.

~~~ http-message
{::include examples/single-notification/request.http}
~~~
{: sourcecode-name="single-notification-request-example.http" #single-notification-request-example title="Single Event Notification Request"}

## Response {#single-notification-response}

{: #single-notification-response-close}
When providing a single notification, the server MUST close the connection immediately after transmitting the event-notification.

~~~ http-message
{::include examples/single-notification/response.http}
~~~
{: sourcecode-name="single-notification-response-example.http" #single-notification-response-example title="Single Event Notification Response"}

{:aside #guidance--no-events-long-lived-resources}
> **Implementation Guidance**
>
> Implementations are advised against sending event-notifications for long-lived resources over HTTP. A resource might be considered long-lived, if a server determines that the resource is unlikely to change in the duration of the notification response. In such instances, resource servers are strongly advised to respond with the `Cache-Control` ({{HTTP-CACHING, Section 5}}) header field and the `max-age` parameter ({{HTTP-CACHING, Section 5.2.2.1}}) set in it.
