# Multiple Notifications {#multiple-notifications}

Instead of long-polling for event notifications, {{&protocol}} can also be used initiate a stream of multiple event notifications.

## Request {#multiple-notifications-request}

To request a stream of event notifications from a resource, a client:
+ MUST use the Events Query Data Model ({{data-model}}) in an appropriate media-type when issuing a subscription request using the `QUERY` method ({{HTTP-QUERY, Section 3}}).
+ MUST specify the +events+ property ({{data-model-property-events}}) in the body of the subscription query.
+ MAY specify any header field under the +events+ property in the body of the subscription query.

A client can also negotiate the form of the representation that encapsulates the event notifications using header fields. Since the response carries an encapsulating representation, header fields can no longer be used to negotiate the form of an event notification itself like in the case of a {{single-notification-request}}.

~~~ http-message
{::include examples/multiple-notifications/request.http}
~~~
{: sourcecode-name="multiple-notifications-request-example.http" #multiple-notifications-request-example title="Multiple Notifications Request"}

## Response {#multiple-notifications-response}

The response stream encapsulates multiple event notifications (typically, but not necessarily) in a composite media-type. We shall be using `application/http` media-type ({{-HTTP1, Section 10.2}}) for the purpose of illustration.

~~~ http-message
{::include examples/multiple-notifications/response.http}
~~~
{: sourcecode-name="multiple-notifications-response-example.http" #multiple-notifications-response-example title="Multiple Notifications Response"}

### Termination {#multiple-notifications-response-termination}

Apart from the connection exceeding time period set in the =duration= key of the =Events= header field, a server MUST end the response immediately after the resource has been deleted.
