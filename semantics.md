
# OpenC2 conceptual model

This section is meant to specify guidelines that will help ensure that OpenC2 implementations are semantically compatible, i.e. different implementations do not refer to or rely on concepts that other implementations are not capable of representing.

The core of OpenC2 is the *message*, which can be:

- A *command*, usually issued from a security orchestrator
- A *response*, a reply to a command (synchronous)
- An *alert*, information usually sent to a security orchestrator (asynchronous)


## OpenC2 Commands

A command consists of:

- An *action* (required), the 'verb' of a command. Example actions include: "deny", "query", "start".
- A *target* (required), the 'subject' of a command. Example targets include: IP addresses, specific users on a system, specific files
- An *actuator* (optional), which implements the action on the target. An actuator generally refers to the capabilities of a system component, not necessarily a particular physical device. Example actuators include: firewalls, anti-malware tools, routers. A "high level" security orchestrator may leave the actuator unspecified, permitting "lower level" components of system to determine the best way to implement the action.
- *Modifiers* (optional), which provide further information about an action. Modifiers are logically bound to actions but not targets or actuators, i.e. a modifier that only makes sense in the context of a particular target or actuator is not permitted. Examples of modifiers include UUIDs, timestamps. The order of modifiers is not part of their semantics, e.g. UUID=..., time=... is equivalent to time=..., UUID=...

OpenC2 commands have two interpretations:

- As a multimethod, with the action dispatching on the types of the target and actuator with the response acting as the return value
    - Usually synchronous
- As a message in the actor model, with the action sent as the message to the actuator with the target as an argument
    - Usually asynchronous


## Namespaces, types and profiles

Targets, actuators, responses and alerts are all *typed*. To avoid nameclashes, these types are *namespaced* with reverse-DNS notation. For the Example Company to refer to a `firewall` type in their `project` namespace, the following would be used:

    com.example.project:firewall

The colon (':') as a namespace delimiter appears to be the rough consensus among exiting OpenC2 implementations/documentation.

The "org.openc2" namespaces are preferred and are maintained by the OpenC2 forum.

Targets, actuators, responses and alerts are all effectively record types (also called structs) and consist of a type and *specifiers* (named fields). For example, an IP address target type, as defined by the Example Company, could be represented as:

    type: com.example.project:ip-address
    specifiers:
        version: "ipv4"
        addresss: "1.2.3.4"

Further prototype implementations will likely reveal the need for more precisely/strictly defined requirements for legal type and field names. A 'safe' assumption is that field and type names will be restricted to legal identifier/variable names in common programming languages.

A target or actuator *profile* is a human-readable specification that defines a type, the namespace that it is part of, the types of responses and/or alerts that may be issued in response to a command, and, in the case of actuator profiles, the action/target combinations that the actuator is compatible with. Similarly, response/alert profiles define response and alert types.

Note that record types have a natural subtyping relation; a type MAY subtype another type if its specifiers are a superset of the original type. Profiles SHOULD explicitly note that they are subtyping an existing profile if this is intended.

Further prototype implementations will likely reveal the need for more precisely/strictly defined requirements for target and actuator profiles.

It is suggested that the OpenC2 forum define minimalist, *generic* response/alert types that are issued in response to commands that do not specify an actuator. *Errors*, e.g. in response to an unsupported or invalid command, are special cases of responses and/or alerts.

It is suggested that the generic response types be defined under the `org.openc2.response` namespace and that errors be defined under the `org.openc2.error` namespace. Prototype implementations may reveal a need for generic alerts under a separate `org.openc2.alert` namespace.

The data types that may be used as target and actuator specifiers are defined in the following section. Profiles may define further restrictions, e.g. that a string conforms to a particular regular expression, that a number appears between a certain range, that keys and/or values of a map data type are all of a particular datatype. Types may have *dependencies*, i.e. valid values for a field may depend on other fields, e.g. requiring the length of a list to equal a number specified in a different field. The dependencies MUST be totally orderable, i.e. no cyclic dependencies. This roughly corresponds to the theoretical notion of dependent record types.

Implementations SHOULD reject OpenC2 commands that contain target or actuator specifiers that do not conform to their corresponding profiles. OpenC2 commands that are rejected SHOULD issue an appropriate error response.


## OpenC2 Data types

### Primitive data types

- Number (IEEE 754 64-bit precision)
- String (UTF-8, no maximum length, but implementations may set limits on overall OpenC2 message sizes)
- Boolean (true, false)
- Null

### Composite data types

- Map (i.e. an associative array, unordered collection of key-value pairs; keys and values may be any OpenC2 data type)
- Vector (i.e. an array, ordered collection of values; values may be any OpenC2 data type)

