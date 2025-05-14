# Including Representation {#representation-with-notifications}

{{&protocol}} allows a user agent to simultaneously request for a representation along with event notifications. This not only saves on an extra round trip, but relieves a user agent from the burden of ensuring that event notifications are temporally coordinated with the representation.

## Request {#representation-request}

To request that a representation of the resource be sent along with events notifications, a client:
+ MUST use the Events Query Data Model ({{data-model}}) in an appropriate media-type when issuing a subscription request using the `QUERY` method ({{HTTP-QUERY, Section 3}}).
+ MUST specify the +state+ property ({{data-model-property-state}}) in the body of the subscription query.
+ MAY specify any header field under the +state+ property in the body of the subscription query.

Otherwise, the request is identical to a [Multiple Notifications Request](#multiple-notifications-request):

~~~ http-message
{::include examples/representation/request.http}
~~~
{: sourcecode-name="representation-request-example.http" #representation-request-example title="Representation and Notifications Request"}

## Response {#representation-response}

Much like in the case of [Multiple Notifications Response](#multiple-notifications-response), the response stream encapsulates the representation (typically) in a composite media-type. Again, we shall use the `application/http` media-type ({{-HTTP1, Section 10.2}}) for the purpose of illustration.

~~~ http-message
{::include examples/representation/response.http}
~~~
{: sourcecode-name="representation-response-example.http" #representation-response-example title="Representation and Notifications Response"}
