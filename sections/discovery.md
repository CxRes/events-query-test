# Discovery {#discovery}

A user agent can discover if a server supports {{&protocol}} on a resource by examining the `Accept-Query` header field ({{HTTP-QUERY, Section 3}}) in a response for an appropriate media-type that can accommodate the {{<<data-model}}.

~~~ http-message
{::include examples/discovery/request.http}
~~~
{: sourcecode-name="discovery-request-example.http" #discovery-request-example title="Discovery Request"}

~~~ http-message
{::include examples/discovery/response.http}
~~~
{: sourcecode-name="discovery-response-example.http" #discovery-response-example title="Discovery Response"}

{:aside #no-events-long-lived-resources}
> **Implementation Advice**
>
> Servers are advised against enabling event-notifications on long-lived resources over HTTP. A resource might be considered long-lived, if a server determines a low probability of an event on the resource in the duration of the response. In such instances, servers are strongly advised to respond with the `Cache-Control` ({{HTTP-CACHING, Section 5}}) header field and the `max-age` parameter ({{HTTP-CACHING, Section 5.2.2.1}}) set in it.
