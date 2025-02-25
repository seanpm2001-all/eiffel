<!---
   Copyright 2017-2021 Ericsson AB and others.
   For a full list of individual contributors, please see the commit history.

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
--->

# EiffelActivityFinishedEvent (ActF)
The EiffelActivityFinishedEvent declares that a previously started activity (declared by [EiffelActivityTriggeredEvent](./EiffelActivityTriggeredEvent.md) followed by [EiffelActivityStartedEvent](./EiffelActivityStartedEvent.md)) has finished.

## Data Members
### data.outcome
__Type:__ Object  
__Required:__ Yes  
__Description:__ The outcome of the activity.

#### data.outcome.conclusion
__Type:__ String  
__Required:__ Yes  
__Legal values:__ SUCCESSFUL, UNSUCCESSFUL, FAILED, ABORTED, TIMED_OUT, INCONCLUSIVE  
__Description:__ A terse standardized conclusion of the activity, designed to be machine readable.  
SUCCESSFUL signifies that the activity was concluded and the outcome matched expectations.  
UNSUCCESSFUL signifies that the activity was concluded, but the outcome did not match expectations. To exemplify, a compilation job was successfully invoked, but compilation failed.  
FAILED signifies that the activity could not be successfully executed. To exemplify, a compilation could not be invoked, e.g. due to misconfiguration or environment issues.  
ABORTED signifies that the activity was aborted before it could be concluded.  
TIMED_OUT signifies that the activity did not conclude within the allowed time frame.  
INCONCLUSIVE signifies that the outcome of the activity could not be determined.

#### data.outcome.description
__Type:__ String  
__Required:__ No  
__Description:__ A verbose description of the activity outcome, designed to provide human readers with further information.

### data.persistentLogs
__Type:__ Object[]  
__Required:__ No  
__Description:__ An array of persistent log files generated during execution.

#### data.persistentLogs.mediaType
__Type:__ String  
__Required:__ No  
__Description:__ The [media type](https://en.wikipedia.org/wiki/Media_type) of the URI's payload. Can be used to differentiate between various representations of the same log, e.g. text/html for human consumption and text/plain or application/json for the machine-readable form.

#### data.persistentLogs.name
__Type:__ String  
__Required:__ Yes  
__Description:__ The name of the log file.

#### data.persistentLogs.tags
__Type:__ String[]  
__Required:__ No  
__Description:__ Arbitrary tags and keywords that describe this log.

#### data.persistentLogs.uri
__Type:__ String  
__Required:__ Yes  
__Description:__ The URI at which the log can be retrieved.

## Links

This section describes which link types are valid for this event type. For details on how to express the link objects themselves see [The Links Object](../eiffel-syntax-and-usage/the-links-object.md).

### ACTIVITY_EXECUTION
__Required:__ Yes  
__Legal targets:__ [EiffelActivityTriggeredEvent](../eiffel-vocabulary/EiffelActivityTriggeredEvent.md)  
__Multiple allowed:__ No  
__Description:__ Declares the activity execution that was finished. In other words, [EiffelActivityTriggeredEvent](../eiffel-vocabulary/EiffelActivityTriggeredEvent.md) acts as a handle for the activity execution. This differs from __CONTEXT__. In __ACTIVITY_EXECUTION__ the source carries information pertaining to the target (i.e. the activity started, finished or was canceled). In __CONTEXT__, on the other hand, the source constitutes a subset of the target (e.g. this test case was executed as part of that activity or test suite).

### CAUSE
__Required:__ No  
__Legal targets:__ Any  
__Multiple allowed:__ Yes  
__Description:__ Identifies a cause of the event occurring. SHOULD not be used in conjunction with __CONTEXT__: individual events providing __CAUSE__ within a larger context gives rise to ambiguity. It is instead recommended to let the root event of the context declare __CAUSE__.  

### CONTEXT
__Required:__ No  
__Legal targets:__ [EiffelActivityTriggeredEvent](../eiffel-vocabulary/EiffelActivityTriggeredEvent.md),
[EiffelTestSuiteStartedEvent](../eiffel-vocabulary/EiffelTestSuiteStartedEvent.md)  
__Multiple allowed:__ No  
__Description:__ Identifies the activity or test suite of which this event constitutes a part.

### FLOW_CONTEXT
__Required:__ No  
__Legal targets:__ [EiffelFlowContextDefinedEvent](../eiffel-vocabulary/EiffelFlowContextDefinedEvent.md)  
__Multiple allowed:__ Yes  
__Description:__ Identifies the flow context of the event: which is the continuous integration and delivery flow in which this occurred – e.g. which product, project, track or version this is applicable to.

## Meta Members
### meta.id
__Type:__ String  
__Format:__ [UUID](http://tools.ietf.org/html/rfc4122)  
__Required:__ Yes  
__Description:__ The unique identity of the event, generated at event creation.

### meta.type
__Type:__ String  
__Format:__ An event type name  
__Required:__ Yes  
__Description:__ The type of event. This field is required by the recipient of the event, as each event type has a specific meaning and a specific set of members in the __data__ and __links__ objects.

### meta.version
__Type:__ String  
__Format:__ [Semantic Versioning 2.0.0](http://semver.org/spec/v2.0.0.html)  
__Required:__ Yes  
__Description:__ The version of the event type. This field is required by the recipient of the event to interpret the contents. Please see [Versioning](../eiffel-syntax-and-usage/versioning.md) for more information.

### meta.time
__Type:__ Integer  
__Format:__ UNIX Epoch time, in milliseconds.  
__Required:__ Yes  
__Description:__ The event creation timestamp.

### meta.tags
__Type:__ String[]  
__Format:__ Free text  
__Required:__ No  
__Description:__ Any tags or keywords associated with the events, for searchability purposes.

### meta.source
__Type:__ Object  
__Format:__  
__Required:__ No  
__Description:__ A description of the source of the event. This object is primarily for traceability purposes, and while optional, some form of identification of the source is __HIGHLY RECOMMENDED__. It offers multiple methods of identifying the source of the event, techniques which may be select from based on the technology domain and needs in any particular use case.

#### meta.source.domainId
__Type:__ String  
__Format:__ Free text  
__Required:__ No  
__Description:__ Identifies the [domain](../eiffel-syntax-and-usage/glossary.md#domain) that produced an event.

#### meta.source.host
__Type:__ String  
__Format:__ Hostname  
__Required:__ No  
__Description:__ The hostname of the event sender.

#### meta.source.name
__Type:__ String  
__Format:__ Free text  
__Required:__ No  
__Description:__ The name of the event sender.

#### meta.source.serializer
__Type:__ String  
__Format:__ [purl specification](https://github.com/package-url/purl-spec)  
__Required:__ No  
__Description:__ The identity of the serializer software used to construct the event, in [purl format](https://github.com/package-url/purl-spec).

#### meta.source.uri
__Type:__ String  
__Format:__ URI  
__Required:__ No  
__Description:__ The URI of, related to or describing the event sender.

### meta.security
__Type:__ Object  
__Format:__  
__Required:__ No  
__Description:__ An optional object for enclosing security related information, particularly supporting data integrity. See [Security](../eiffel-syntax-and-usage/security.md) for further information.

#### meta.security.authorIdentity
__Type:__ String  
__Format:__ [Distinguished Name](https://tools.ietf.org/html/rfc2253)  
__Required:__ Yes  
__Description:__ The identity of the author of the event. This property is intended to enable the recipient to identify the author of the event contents and/or look up the appropriate public key for decrypting the __meta.security.integrityProtection.signature__ value and thereby verifying author identity and data integrity.

#### meta.security.integrityProtection
__Type:__ Object  
__Format:__  
__Required:__ No  
__Description:__ An optional object for enabling information integrity protection via cryptographic signing. To generate a correct __meta.security.integrityProtection__ object:
1. Generate the entire event, but with the __meta.security.integrityProtection.signature__ value set to an empty string.
1. Serialize the event on [Canonical JSON Form](https://tools.ietf.org/html/draft-staykov-hu-json-canonical-form-00).
1. Generate the signature using the __meta.security.integrityProtection.alg__ algorithm.
1. Set the __meta.security.integrityProtection.signature__ value to the resulting signature while maintaining Canonical JSON Form.
To verify the integrity of the event, the consumer then resets __meta.security.integrityProtection.signature__ to an empty string and ensures Canonical JSON Form before verifying the signature.

##### meta.security.integrityProtection.alg
__Type:__ String  
__Format:__ [A valid JWA RFC 7518 alg parameter value](https://tools.ietf.org/html/rfc7518#section-3.1), excluding "none"  
__Required:__ Yes  
__Description:__ The cryptographic algorithm used to digitally sign the event. If no signing is performed, the __meta.security.integrityProtection__ SHALL be omitted rather than setting __meta.security.integrityProtection.alg__ to "none".

##### meta.security.integrityProtection.signature
__Type:__ String  
__Format:__  
__Required:__ Yes  
__Description:__ The signature produced by the signing algorithm.

##### meta.security.integrityProtection.publicKey
__Type:__ String  
__Format:__  
__Required:__ No  
__Description:__ The producer of the event may include the relevant public key for convenience, rather than relying a separate key distribution mechanism. Note that this property, along with the rest of the event, is encompassed by the integrity protection offered via __meta.security.integrityProtection__.

#### meta.security.sequenceProtection
__Type:__ Object[]  
__Format:__  
__Required:__ No  
__Description:__ An optional object for enabling verification of intact event sequences in a distributed environment, thereby protecting against data loss, race conditions and replay attacks. It allows event publishers to state the order in which they produce a certain set of events. In other words, it cannot provide any global guarantees as to event sequencing, but rather per-publisher guarantees. Every object in the array represents a named sequence of which this event forms a part. For every event including a given named sequence, the publisher SHALL increment __meta.security.sequenceProtection.position__ by 1. The first event produced in a given named sequence SHALL numbered 1.

##### meta.security.sequenceProtection.sequenceName
__Type:__ String  
__Format:__  
__Required:__ Yes  
__Description:__ The name of the sequence. There MUST not be two identical __meta.security.sequenceProtection.sequenceName__ values in the same event.

##### meta.security.sequenceProtection.position
__Type:__ Integer  
__Format:__  
__Required:__ Yes  
__Description:__ The number of the event within the named sequence.

## Version History
| Version   | Introduced in                                          | Changes                                 |
| --------- | ------------------------------------------------------ | --------------------------------------- |
| 3.2.0     | [edition-lyon](../../../tree/edition-lyon)             | Add links.domainId member (see [Issue 233](https://github.com/eiffel-community/eiffel/issues/233)). |
| 3.1.0     | No edition set                                         | Add `data.persistentLogs.{mediaType,tags}`.   |
| 3.0.0     | [edition-agen](../../../tree/edition-agen)             | Improved information integrity protection | (see [Issue 185](https://github.com/eiffel-community/eiffel/issues/185)) |
| 2.0.0     | [dc5ec6f](../../../blob/dc5ec6fb87e293eeffe88fdafe698eec0f5a2c89/eiffel-vocabulary/EiffelActivityFinishedEvent.md) | Introduced purl identifiers instead of GAVs (see [Issue 182](https://github.com/eiffel-community/eiffel/issues/182)) |
| 1.1.0     | [edition-toulouse](../../../tree/edition-toulouse)     | Multiple links of type FLOW_CONTEXT allowed. |
| 1.0.0     | [edition-bordeaux](../../../tree/edition-bordeaux)     | Initial version.                        |

## Examples
* [Simple example](../examples/events/EiffelActivityFinishedEvent/simple.json)
