# IANA Considerations {#iana-considerations}

The change controller for the following registrations is: "IETF (iesg@ietf.org) - Internet Engineering Task Force".

## HTTP Field Registration {#field-registration}

IANA is requested to add the following entry in the "[Hypertext Transfer Protocol (HTTP) Field Name Registry](https://www.iana.org/assignments/http-fields/)" defined by [HTTP]:

| Header Field Names  | Status     | Structured-Type  | Reference         |
|-
| =Events=            | Permanent  | Dictionary       | {{events-field}}  |

## The HTTP Events Field Registry {#events-field-registry}

IANA is requested to create a new registry, "HTTP Events Field Registry", under the [Hypertext Transfer Protocol (HTTP) Parameters](https://www.iana.org/assignments/http-parameters/) registry to register properties for use in the =Events= header field. New registrations will use the Specification Required policy ({{RFC8126, Section 4.6}}).

### Template {#events-field-registry-template}

The registration template for the "HTTP Events Field Registry" is:

+ Property Name: A Dictionary ({{HTTP-SF, Section 3.2}}) key to be used in the =Events= header field.

+ Structured Type: The Structured Data Type of the value associated with the key, according to requirements in {{Section 3.2 of HTTP-SF}}.

+ Optional Parameters: An enumeration of optional parameters, and their values, associated with the entry.

+ Description:

+ Reference:

+ Notes: (optional)

### Initial Registry Contents {#events-field-registry-initial-content}

The initial contents of the HTTP Events Field Registry are:

| Property Name  | Structured-Type  | Description                | Reference             |
|-
| =duration=     | Integer Item     | See {{duration-property}}  | {{duration-property}} |

<!--
## HTTP Status Code Registration {#status-code-registration}

IANA is requested to add the following entry in the "[Hypertext Transfer Protocol (HTTP) Status Code Registry](https://www.iana.org/assignments/http-status-codes/)" defined by [HTTP]:

| Status Code    | Description         |  Reference          |
|-
| 555            | Connection-Timeout  |                     |
-->
