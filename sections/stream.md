# Notifications Stream {#stream}

{: #stream-description}
{{&protocol}} can also be used to request a stream ({{RFC6202, Section 3}}) of multiple event-notifications.

## Request {#stream-request}

{: #stream-request-procedure}
To request a stream of event-notifications from a resource in an {{&protocol}}, a client MUST express their interest in receiving the event-notifications in a preferred form using a realization of the subscription data model with the `QUERY` method ({{HTTP-QUERY, Section 3}}).

{: #stream-request-conneg}
Since the response transmits event-notifications within an encapsulating representation (See {{stream-response}}), it follows that header fields cannot be used to negotiate the form of event-notifications like in the case of a [Single Notification Request](#single-notification-request){:noabbrev}. Instead, header fields are useful for negotiating the representation that encapsulates event-notifications.

{: #stream-request-example-description}
The following example shows a subscription request for a stream of event-notifications transmitted using the `application/http` media-type. The `events` property indicates the interest in receiving event-notifications. Preferences are specified using the request headers via the `events` property.

~~~ http-message
{::include examples/stream/events-request.http}
~~~
{: sourcecode-name="stream-request-example.http" #stream-request-example title="Notifications Stream Request"}

## Response {#stream-response}

{: #stream-response-encapsution}
The response stream transmits multiple event-notifications in an encapsulating media type. We shall be using `application/http` media-type ({{-HTTP1, Section 10.2}}) for the purpose of illustration.

### Headers {#stream-response-headers}

{: #stream-response-headers-list}
A server able to provide a stream of event-notifications immediately sends the headers which MUST include:

+ {: #stream-response-events-field}
The =Events= header field to communicate the properties of the notifications stream.

    + {: #stream-response-duration-property}
    The =duration= property set to the maximum duration for which the server intends to serve event-notifications.

+ {: #stream-response-incremental-field}
The `Incremental` header field ({{INCREMENTAL-HTTP-MESSAGES, Section 3}}) set to `?1` to indicate that the response is to be immediately forwarded by intermediaries and not buffered.

~~~ http-message
{::include examples/stream/response-headers.http}
~~~
{: sourcecode-name="stream-response-headers-example.http" #stream-response-header-example title="Notifications Stream Response Headers"}

### Notifications {#stream-response-body}

{: #stream-response-event}
Subsequently, when event(s) occur, the server transmits a notification identical to the [Single Notification Response](#single-notification-response){:noabbrev}, except fields redundant with response headers ({{stream-response-headers}}) are omitted.

~~~ http-message


{::include examples/notifications/update.http}
~~~
{: sourcecode-name="stream-update-event.http" #stream-update-event title="Update Notification"}

{: #stream-response-terminal-event}
A server MUST end the response immediately after transmitting the event-notification upon the deletion of a resource. The notification for a delete event might be as follows:

~~~ http-message


{::include examples/notifications/delete.http}
~~~
{: sourcecode-name="stream-delete-event.http" #stream-delete-event title="Delete Notification"}

{: #stream-response-timeout}
Otherwise, a server MUST end the response when the connection duration has exceeded the period set in the =duration= property of the =Events= header field. A server MAY terminate the response earlier.
