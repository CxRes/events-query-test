# Preliminaries {#preliminaries}

## `Events` Header Field {#events-field}

{: #events-field-request}
The =Events= header field MAY be used by a client in request to communicate preferences for the {{&protocol}} response. The =Events= header field is not meant for content negotiation.

{: #events-field-response}
The =Events= header field MUST be used by a server to communicate the properties of a response carrying event notifications.

{: #events-field-stipulations}
=Events= is a Dictionary structured header field ({{HTTP-SF, Section 3.2}}). The order of keys is insignificant. Senders SHOULD NOT generate keys not registered with the HTTP Event Field Registry. Recipients MAY ignore keys that they are unaware of.

*[=Events=]: #events-field (((events (header field) ))) `Events`

### The `duration` property {#duration-property}

{: #duration-property-request}
The =duration= property MAY be used by a client in a request to specify the duration for which they prefer the response stream to remain open. A server is completely free to ignore this property.

{: #duration-property-response}
The =duration= property MUST be used by a server in a response to indicate the minimum duration for which a server intends to keep the response stream open. This property is merely advisory, and a server might still close the response stream before this duration.

{: #duration-property-stipulations}
The =duration= property is a key specified in the =Events= header field of the type Integer ({{HTTP-SF, Section 3.3.1}}). It is used to indicate duration in seconds. Only positive integer values are valid. A value of `0` indicates an indefinite duration. If the value of the =duration= property fails to conform to these stipulations, the key MUST be ignored.

*[=duration=]: #duration-property (((duration (property) ))) `duration`

## `Incremental` Header Field {#incremental-header}

A server providing event notifications using the {{&protocol}} MUST include in the response the `Incremental` header field ({{INCREMENTAL-HTTP-MESSAGES, Section 3}}) with its value set to `?0`.

## Events Query Data Model {#data-model}

{: #data-model-description}
The Events Query Data Model defines the semantics of {{&protocol}} request for multiple notifications and/or representation. It allows for the negotiation of notifications and representation independent of a composite media-type that might be used in a {{&protocol}} response.

{: #data-model-properties}
It has the following OPTIONAL properties:

{:#data-model-property-events}
+ `events` used to specify header fields needed to negotiate notifications, and

{:#data-model-property-state}
+ `state` used to specify header fields needed to negotiate a representation.

{: #data-model-extensions}
Implementations are free to extend the Events Query Data Model with additional properties. Implementations can choose an appropriate media-type for their application to realize the Events Query Data Model.

~~~ yaml
state:
  Accept: text/html
events:
  Accept: example/event-response
~~~
{: sourcecode-name="multiple-events-request-example.yaml" #data-model-example title="Events Query Data Model in YAML syntax"}

*[Events Query Data Model]: #data-model

*[+events+]: #data-model-property-events (((events (property) ))) `events`
*[+state+]: #data-model-property-state (((state (property) ))) `state`
