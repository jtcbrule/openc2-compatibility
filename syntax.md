
# OpenC2 serializations

An OpenC2 *serialization* refers to the syntax of an OpenC2 command. ("What does an OpenC2 command look like 'on the wire'?") In general, this can by any one-to-one mapping from an abstract OpenC2 message to a concrete representation.

A command is of the form:

    ACTION TARGET [ACTUATOR] [MODIFIER=value]...

Where:

- An `ACTION` is an OpenC2 action.
- A `TARGET` consists of a properly namespaced type and specifiers (key-value pairs) that conform to the corresponding target profile.
- An (optional) `ACTUATOR` consists of a properly namespaced type and specifiers that conform to the actuator profile. If present, the actuator MUST be compatible with the `ACTION`/`TARGET` pair, as defined in the actuator profile.
- Zero or more `MODIFIER`s (key-value pairs), which are compatible with the `ACTION`.

A response consists of a properly namespaced type and specifiers that conform to the corresponding response profile. If the response is being issued in response to a command that does not include an `ACTUATOR`, the response type SHOULD be one of the generic OpenC2 responses. If the original command included an `ACTUATOR` then the response type SHOULD be one of the possible response types listed in the actuator profile.

An alert is effectively a response that is issued asynchronously.

The *default* OpenC2 serialization is as JSON (suggest this be referred to as `org.openc2.serialization:json`). Commands are of the form:

    {"action": <action>,
     "target": {"type": <target type>,
                "specifiers": {<target specifiers>}},
     "actuator": {"type": <actuator type>,
                  "specifiers": {actuator specifiers>}},
     "modifiers": {<modifiers>}}

Responses and alerts are of the form:

    {"type": <response type>,
     "specifiers": {<response specifiers>}}

Aside: Implementers will likely find a formal grammar, e.g. in Backus-Naur form useful.

Alternative serializations (e.g. binary representations) MAY be configured manually for implementations that benefit from alternative representations or MAY be negotiated as part of the OpenC2 configuration protocol (see [communication.md](communication.md)). Serializations are referred to using the same namespace system used for targets, actuators, responses and alert types. It is suggested that the officially supported serializations be namespaced under `org.openc2.serialization`.

Ideally, further prototype implementations will establish a rough consensus on the most useful serializations to be officially supported.

