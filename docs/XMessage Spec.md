---
id: XMessageSpec
title: XMessage Specification
sidebar_label: XMessage Specification
---

## 1. Introduction

The XMessage specification is used by tools in the [Communication package](https://samagra-development.github.io/docs/docs/COConversations) ecosystem. It is a subset of the far larger [XMPP](https://xmpp.org/) and also contains a few additional features not found in the XMPP specification.

The purpose of this specification is to provide a common message description standard that many different kinds of compatible tools can be based on. Using a single, shared message description standard has the following advantages:

1. Users in the Communication package can mix and match tools and reassess which they use based on their changing needs. In particular, they don't get locked in to tools that may become deprecated or for which an attractive replacement becomes available.
2. Tool implementors in the Communication ecosystem can benefit from feedback from a broad range of collaborators when designing new core functionality.
3. Tool implementors in the Communication ecosystem can share core implementations.
4. Enables building a message transport protocol between services.

This document is intended primarily for developers who build message processing engines (Transformers) or rule engines (Orchestrator).

The document assumes at least a fair understanding of XML. It is also useful to refer to [XMPP](https://xmpp.org/) for details about shared features. The same features can be built over json as well.

### 1.1 XML vs JSON

Even though JSON is tempting we chose XML for data interchange for the following reasons,

1. Its is a document markup language which is what most of the text is for various modes of communication
2. It has a schema and allows for validation of the same
3. Building contracts among servers are a breeze
4. It has namespaces which are used when combining different format. (Although we are not using namespaces right now, this spec will evolve into using namespaces for various sections as it expands)

## 2. Structure

The current structure of XMessages is divided into following sections.

- Sender/Reciever Information
- Channel/Provider Information
- Transformers that need to act on this message
- Context related parameters
- Message body

Let's dive deeper into all of these, but before that, this is the complete xml schema (this is used for validation currently)

```xml
<?xml version="1.0"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/xmlSchema"
           targetNamespace="https://www.samagra.io"
           xmlns:sam="https://www.samagra.io"
           elementFormDefault="qualified">

    <xs:simpleType name="timestamp">
        <xs:restriction base="xs:negativeInteger">
            <xs:pattern value="^(\d{13})?$"/>
        </xs:restriction>
    </xs:simpleType>

    <xs:complexType name="messageID">
        <xs:sequence>
            <xs:element name="id" type="xs:string"/>
            <xs:element name="gupshupMessageID" type="xs:string"/>
            <xs:element name="whatsappMessageID" type="xs:string"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="thread">
        <xs:sequence>
            <xs:element name="offset" type="xs:positiveInteger"/>
            <xs:element name="startDate" type="sam:timestamp"/>
            <xs:element name="lastMessage" type="sam:messageID"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="senderReceiverInfo">
        <xs:sequence>
            <xs:element name="bot" type="xs:boolean"/>
            <xs:element name="broadcast" type="xs:boolean"/>
            <xs:element name="userID" type="xs:string"/>
            <xs:element name="campaignID" type="xs:string"/>
            <xs:element name="formID" type="xs:string"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="conversationStage">
        <xs:sequence>
            <xs:element name="stage" type="xs:string"/>
            <xs:element name="state" type="xs:string"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="address">
        <xs:sequence>
            <xs:element name="city" type="xs:string"/>
            <xs:element name="country" type="xs:string"/>
            <xs:element name="countryCode" type="xs:positiveInteger"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="contactCard">
        <xs:sequence>
            <xs:element name="address" type="sam:address"/>
            <xs:element name="name" type="xs:string"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="locationParams">
        <xs:sequence>
            <xs:element name="latitude" type="xs:double"/>
            <xs:element name="longitude" type="xs:double"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="messageMedia">
        <xs:sequence>
            <xs:element name="category" type="xs:string"/>
            <xs:element name="text" type="xs:string"/>
            <xs:element name="url" type="xs:string"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="xMessagePayload">
        <xs:sequence>
            <xs:element name="text" type="xs:string"/>
            <xs:element name="media" type="sam:messageMedia"/>
            <xs:element name="location" type="sam:locationParams"/>
            <xs:element name="contactCard" type="sam:contactCard"/>
        </xs:sequence>
    </xs:complexType>

    <xs:complexType name="transformer">
        <xs:sequence>
            <xs:element name="id" type="xs:string"/>
            <xs:element name="meta" type="xs:anyType"/>
        </xs:sequence>
    </xs:complexType>

    <xs:element name="xMessage">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="app" type="xs:string"/>
                <xs:element name="messageID" type="xs:string"/>
                <xs:element name="channelURI" type="xs:string"/>
                <xs:element name="providerURI" type="xs:string"/>
                <xs:element name="timestamp" type="sam:timestamp"/>
                <xs:element name="userstate" type="xs:string"/>
                <xs:element name="encryptionProtocol" type="xs:string"/>
                <xs:element name="thread" type="sam:thread"/>
                <xs:element name="to" type="sam:senderReceiverInfo"/>
                <xs:element name="from" type="sam:senderReceiverInfo"/>
                <xs:element name="payload" type="sam:xMessagePayload"/>
                <xs:element name="conversationStage" type="sam:conversationStage"/>
                <xs:element name="messageState" type="xs:string"/>
                <xs:element name="lastMessageID" type="xs:string"/>
                <xs:element name="transformers" type="sam:transformer"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

### 2.1 Sender/Reciever Information

The `from` and `to` construct uses the structure show below.

```xml
<xs:complexType name="senderReceiverInfo">
    <xs:sequence>
        <xs:element name="bot" type="xs:boolean"/>
        <xs:element name="broadcast" type="xs:boolean"/>
        <xs:element name="userID" type="xs:string"/>
        <xs:element name="campaignID" type="xs:string"/>
        <xs:element name="formID" type="xs:string"/>
    </xs:sequence>
</xs:complexType>
```

| Parameter | Description |
| --- | --- |
| `bot` | Refers to if the response/reply from/to the user is from a bot or an actual user |
| `broadcast` | Is for use case where the user or sever is sending the same message to multiple users or a single one |
| `userID` | Current User ID; When the message is still not parsed, it could be his phone number but should generally be a UUID which represents the user in the database |
| `campaignID` | ID of the conversation campaign the message is part of |
| `formID` | Ff the reply is for a ODK Form based bot, this represents the ID of that form |

### 2.2 Channel/Provider Information

```xml
<xs:element name="channelURI" type="xs:string"/>
<xs:element name="providerURI" type="xs:string"/>
```

| Parameter     | Description                                                |
| ------------- | ---------------------------------------------------------- |
| `contentURI`  | Channel for the message (SMS, Email, WhatsApp etc)         |
| `providerURI` | Provider for the message (Gupshup, AmazonSES, Twilio, etc) |

### 2.3 Transformers that need to act on this message

Each message can be acted on by multiple transformers to modify/reply the payload/channel etc. This happens through `Transformers`. Each transformer is assigned an ID when it is registered.

```xml
<xs:complexType name="transformer">
    <xs:sequence>
        <xs:element name="id" type="xs:string"/>
        <xs:element name="meta" type="xs:anyType"/>
    </xs:sequence>
</xs:complexType>
```

| Parameter | Description                                               |
| --------- | --------------------------------------------------------- |
| `id`      | Transformer's ID (genereated at the time of registration) |
| `meta`    | Meta data for the transformer                             |

### 2.4 Context related parameters

This section describes the

```xml
<xs:element name="app" type="xs:string"/>
<xs:element name="userstate" type="xs:string"/>
<xs:element name="conversationStage" type="sam:conversationStage"/>
<xs:element name="messageState" type="xs:string"/>
<xs:element name="lastMessageID" type="xs:string"/>
<xs:element name="thread" type="sam:thread"/>
```

| Parameter | Description |
| --- | --- |
| `app` | App is a global namespace the message is part of. For example an app may correspond to multichannel conversation that many users are part of. Any segreation of conversation is defined as an app |
| `userstate` | Any of the following => `typing`, `offline`, `unregistered`, `registered`, `active`, `inactive` |
| `messageState` | State of a message sent to the user. Any of the following => `FAILED_TO_DELIVER`, `DELIVERED`, `QUEUED`, `READ`, `REPLIED` |
| `conversationStage` | Stage of the current conversation. Any of the following => `STARTING`, `ONGOING`, `COMPLETED` |
| `lastMessageID` | ID of the last message sent for this conversation thread. |
| `thread` | The thread which the message belongs to |

### 2.5 Message body

Message body referes to the actual message body. The current list of message types includes plain text, xHTML, location, media, or a contactCard.

```xml

<xs:complexType name="contactCard">
    <xs:sequence>
        <xs:element name="address" type="sam:address"/>
        <xs:element name="name" type="xs:string"/>
    </xs:sequence>
</xs:complexType>

<xs:complexType name="locationParams">
    <xs:sequence>
        <xs:element name="latitude" type="xs:double"/>
        <xs:element name="longitude" type="xs:double"/>
    </xs:sequence>
</xs:complexType>

<xs:complexType name="messageMedia">
    <xs:sequence>
        <xs:element name="category" type="xs:string"/>
        <xs:element name="text" type="xs:string"/>
        <xs:element name="url" type="xs:string"/>
    </xs:sequence>
</xs:complexType>

<xs:complexType name="xMessagePayload">
    <xs:sequence>
        <xs:element name="text" type="xs:string"/>
        <xs:element name="media" type="sam:messageMedia"/>
        <xs:element name="location" type="sam:locationParams"/>
        <xs:element name="contactCard" type="sam:contactCard"/>
    </xs:sequence>
</xs:complexType>

<xs:element name="payload" type="sam:xMessagePayload"/>
```

## 3. Styling

There are two supported protocols for message styling - [XEP 0393](https://xmpp.org/extensions/xep-0393.html) and [XEP 0071](https://xmpp.org/extensions/xep-0071.html). Note only message inside the `<text>` are to be implemented with the following rules. All other tags are escaped. The common use cases and examples of messages are as follows:

### 3.1 Bold

```xml
<payload>
    <text>
        (Two spans, both )(*alike in dignity*)
  </text>
</payload>
```

Should be styled as => (Two spans, both )(**alike in dignity**)

### 3.2 Strikethrough

```xml
<payload>
    <text>
        Everyone ~dis~likes cake
  </text>
</payload>
```

Should be styled as => Everyone ~~dis~~likes cake

### 3.3 Italics

```xml
<payload>
    <text>
        The full title is _Twelfth Night, or What You Will_ but _most_ people shorten it.
  </text>
</payload>
```

Should be styled as => The full title is _Twelfth Night, or What You Will_ but _most_ people shorten it.

### 3.4 Monospace

Text enclosed by a '`' (U+0060 GRAVE ACCENT) is a preformatted span SHOULD be displayed inline in a monospace font. A preformatted span may only contain a single plain span.

```xml
<payload>
    <text>
        Wow, I can write in `monospace`!
  </text>
</payload>
```

Should be styled as => Wow, I can write in `monospace`!

### 3.5 Plain

Any text inside of a block that is not part of another span is implicitly considered to be inside of a "plain text" span.

```xml
<payload>
    <text>
        (There are three blocks in this body marked by parens,)
        (but there is no *formatting)
        (as spans* may not escape blocks.)
  </text>
</payload>
```

### 3.6 Escaping

On rare occasions styling hints may conflict with the contents of a message. For example, if the user sends the emoji `"> _ <"` it would be interpreted as a block quote. Senders may indicate to the receiver that a particular message SHOULD NOT be styled by adding an empty `<unstyled>` element.

```xml
<payload>
    <text>
        <unstyled>&gt; _ &lt;</unstyled>
    </text>
</message>
```

### 3.7 Implementation Details

This document does not define a regular grammar and thus styling cannot be matched by a regular expression. Instead, a simple parser can be constructed by first parsing all text into blocks and then recursively parsing the child-blocks inside block quotations, the spans inside individual lines, and by returning the text inside preformatted blocks without modification.

It is RECOMMENDED that styling directives be displayed and formatted in the same manner as the text they apply to. For example, the string "_emphasis_" would be rendered as "_emphasis_".

This specification does not provide a mechanism for removing styling from individual spans or blocks within a styled message. Implementations are free to implement their own workarounds, for example by inserting Unicode non-printable characters to invalidate styling directives, but no specific technique is known to be widely supported.

## 4. FAQs

To be updated based on incoming feedback. Feel free to write into tech@samagragovernance.in in case you have questions, feedback or want to know more!

