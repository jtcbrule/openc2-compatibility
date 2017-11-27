
# OpenC2 communication protocols

In the context of OpenC2, a *communication protocol* is a protocol stack that permits two OpenC2 system components to exchange messages with each other. This includes the necessary specifications of the transport layer, application layer and security protocols.


## Default communication protocol: HTTP/1.1

The default *insecure* OpenC2 messaging protocol is an HTTP/1.1 POST on port 6767. This MUST be a legal HTTP/1.1 request, with the OpenC2 command as the HTTP request message body and the OpenC2 response as the HTTP response message body.

Request-URI SHOULD be "/". Content-type SHOULD be `application/json`, assuming the default OpenC2 JSON serialization. Status-Code and Reason-Phrase SHOULD be "200 OK".

A security orchestrator MAY listen for alerts on port 6768. Alerts are sent as the message body of legal HTTP/1.1 request. The HTTP response message to an OpenC2 alert SHOULD be the empty OpenC2 message (`{}`, assuming the default OpenC2 JSON serialization).

This default communication protocol is suitable for communication between OpenC2 components on a trusted, private network. This protocol MUST NOT be used on untrusted networks. The default secure OpenC2 messaging protocol is over HTTPS with client authentication.

Alternative messaging protocols MAY be configured manually for implementations or MAY be negotiated over the default communication protocol. Messaging protocols are referred to as part of the Openc2 namespace system; it is suggested that the officially supported messaging protocols be namespaced under `org.openc2.communication`.

Ideally, further prototype implementations will establish a rough consensus on the most useful serializations to be officially supported.


## OpenC2 protocol negotiation

Configuration of target, actuator, and response profiles and serialization and communication protocols for an OpenC2 system component MAY be done manually; this is likely desirable in high-security environments. However, the combination of default communication protocol and serialization also permits negotiating the configuration of OpenC2 components via "in band" OpenC2 messages. The following is a sketch of a configuration profile in the `org.openc2.configuration` namespace.

Using the default communication protocol and serialization, an orchestrator sends an OpenC2 command with the `query` action to an OpenC2 actuator. The response contains details of the supported serializations, communication protocols and profiles. This is followed by an OpenC2 `start`, with specifiers that establish the communication protocol that all future communication occurs on.

