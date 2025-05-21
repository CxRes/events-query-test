# Notification Stream {#stream}

{: #stream-description}
Instead of long-polling for event-notifications, {{&protocol}} can also be used request a stream ({{RFC6202, Section 3}}) of multiple event-notifications.

## Request {#stream-request}

{: #stream-request-procedure}
To request a stream of event-notifications from a resource using {{&protocol}}, a client MUST use the subscription data model ({{data-model}}) with the +events+ property in an appropriate media-type when issuing a request using the `QUERY` method ({{HTTP-QUERY, Section 3}}).

{: #stream-request-events-conneg}
The +events+ property MAY be used to specify any header field in the body of the subscription query to negotiate notifications.

{: #stream-request-conneg}
A client can also negotiate the form of the representation that encapsulates the event-notifications using header fields. Since the response carries an encapsulating representation, header fields can no longer be used to negotiate the form of an event-notification itself like in the case of a [Single Notification Request](#single-notification-request).

~~~ http-message
{::include examples/stream/events-request.http}
~~~
{: sourcecode-name="stream-request-example.http" #stream-request-example title="Notifications Stream Request"}

## Response {#stream-response}

{: #stream-response-encapsution}
The response stream encapsulates multiple event-notifications (typically, but not necessarily) in a composite media-type. We shall be using `application/http` media-type ({{-HTTP1, Section 10.2}}) for the purpose of illustration.

### Headers {#stream-response-header}

{: #stream-response-headers-list}
A server able to provide a stream of event-notifications immediately sends the header which MUST include:

+ {: #stream-response-events-field}
The =Events= header field to communicate the properties of the notifications stream.

    + {: #stream-response-duration-property}
    The =duration= property set with the time for which the server intends to serve notifications.

+ {: #stream-response-incremental-field}
The `Incremental` header field ({{INCREMENTAL-HTTP-MESSAGES, Section 3}}) set to `?1` to indicate that the response is to be immediately forwarded by intermediaries and not buffered.

~~~ http-message
{::include examples/stream/response-headers.http}
~~~
{: sourcecode-name="stream-response-headers-example.http" #stream-response-header-example title="Notifications Stream Response Headers"}

### Notifications {#stream-response-body}

{: #stream-response-event}
Subsequently, when event(s) occur, the server transmits a notification identical to the [Single Notification Response](#single-notification-response), except header fields redundant with response header ({{stream-response-header}}) are omitted.

~~~ http-message
{::include examples/notifications/update.http}
~~~
{: sourcecode-name="stream-update-event.http" #stream-update-event title="Update Notification"}

{: #stream-response-terminal-event}
Apart from the connection exceeding time period set in the =duration= property of the =Events= header field, a server MUST end the response immediately after the resource has been deleted.

~~~ http-message
{::include examples/notifications/delete.http}
~~~
{: sourcecode-name="stream-delete-event.http" #stream-delete-event title="Delete Notification"}
