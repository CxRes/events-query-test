# Discovery {#discovery}

A user agent can discover if a server supports {{&protocol}} on a resource by examining the `Accept-Query` header field ({{HTTP-QUERY, Section 3}}) in a response for an appropriate media-type.

~~~ http-message
{::include examples/discovery/request.http}
~~~
{: sourcecode-name="discovery-request-example.http" #discovery-request-example title="Discovery Request"}

~~~ http-message
{::include examples/discovery/response.http}
~~~
{: sourcecode-name="discovery-response-example.http" #discovery-response-example title="Discovery Response"}
