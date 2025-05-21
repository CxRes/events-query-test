# Subscription Data Model {#data-model}

{: #data-model-description}
The abstract data model specifies the semantics of the query that allows a client to negotiate the form of notifications and representation when subscribing for event-notifications.

{: #data-model-properties}
The data model has the following OPTIONAL properties:

+ {: #data-model-property-events}
`events` to specify header fields needed to content negotiate notifications, and

+ {: #data-model-property-state}
`state` to specify header fields needed to content negotiate a representation.

{: #data-model-extensions}
Implementations are free to extend the data model with additional properties. Implementations can choose an appropriate media-type to realize the subscription data model.

~~~ yaml
state:
  Accept: text/html
events:
  Accept: example/event-response
~~~
{: sourcecode-name="data-model-example.yaml" #data-model-example title="Events Query Data Model in a YAML like syntax"}

*[data model]: #data-model
*[Data Model]: #data-model

*[+events+]: #data-model-property-events (((events (property) ))) `events`
*[+state+]: #data-model-property-state (((state (property) ))) `state`
