# UCI Basics

[XMessage Specification](#xmessage-specification)

[Transformer ODK Conversation](#transformer-odk-conversation)

[Creating a new transformer](#creating-a-new-transformer)

[User Segment](#user-segment)

[Adapters Overview and how to add a new adapter](#adapters-overview-and-how-to-add-a-new-adapter)

****

# XMessage Specification
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


# Transformer ODK Conversation


## 1. Overview

A transformer is a stateless processing object that takes inputs and converts the input into a processed response. Transformers  may in turn call external services if needed. This transformer uses odk forms to get the input from user and output the next reply to be sent. ODK forms contains the questions and flow of the questions.   


## 2. Setup ODK Aggregate

[ODK Aggregate](https://docs.getodk.org/aggregate-intro/) is an open source Java application that stores, analyzes, and presents XForm survey data collected using ODK Collect. It supports a wide range of data types, and is designed to work well in any hosting environment.

With Aggregate, your team can:

- Host blank XForms used by ODK Collect or other OpenRosa clients
- Store and manage XForm submission data
- Visualize collected data using maps and simple graphs
- Export and publish data in a variety of formats

Aggregate can be hosted on cloud providers such as DigitalOcean, and Amazon Web Services, or your own local or cloud server. There's also a pre-configured virtual machine image that is ready to deploy on any computer.

Please check this link to find [how to set up ODK Aggregate?](https://docs.getodk.org/aggregate-setup/)

Refer this link to find steps to [use ODK.](https://docs.getodk.org/aggregate-use/)


## 3. How it works:

It saves all the question & assessment data in db and fetch the same when needed. This transformer uses MenuManager class to handle all ODK related processes. 

MenuManager uses [javarosa](https://mvnrepository.com/artifact/org.getodk/javarosa/3.3.0) library. It loads the odk form, fetches the next question based on previous xpath or no path, validate the answer to current question & generate the error messages if any.

A form is first loaded to the ```FECWrapper``` class. It gives us the ```FormEntryController``` class object to handle ODK related functionlity. Some of the main methods we use to fetch the relavent data using this class are given below.

1. Get current form index 

	```java
	FormController.getModel().getFormIndex()
	``` 

2. Set the current index to previous event of the form 

	```java
	FormController.stepToPreviousEvent()
	``` 

3. Set the current index to next event of the form 
	
	```java
	FormController.stepToNextEvent()
	```

4. TreeReference of the fully qualified element of the form 

	```java
	FormController.getModel().getFormIndex().getReference()
	```

5. Get the text for question at current index 

	```java
	FormController.getModel().getQuestionPrompt().getQuestionText()
	```

6. Get the type for question (Eg. select/input/audio/image etc.) 

	```java 
	FormController.getModel().getQuestionPrompt().getControlType()
	```


FormEntryController has different event types based on the ODK form, from which some are listed below.

1. Represents a question: ```EVENT_QUESTION``` 
2. Represents a question group: ```EVENT_GROUP``` 
3. Represents a repeat question: ```EVENT_REPEAT```


## 4. MenuManager Methods

#### 1. start

This method loads the odk form, process the current answer, identify the next question/reponse and returns a ```ServiceResponse``` which includes a few properties that will be used by the ODK Transformer.

1. Current xpath
2. XMessage payload for Next question with question text
3. Updated Instance XML
4. Form Version
5. Form ID
6. Next Question object


#### 2. addResponseToForm

It validates the current answer against the type of question (input/select choice/media etc.) & returns a status for the same.


#### 3. getIndexFromXPath

It gets the index from the give xpath.


#### 4. getXPath

It gets the xpath from the current form index.


#### 5. renderQuestion

Renders a question by cleaning its text & adding help text to it, if there is any.


#### 6. createView

Creates and returns a new view based on the event type passed in. It returns the XMessage payload with question text and its choices if the question is of select choice type.


#### 7. loadForm

It loads a ODK xml form from the given path. It creates form defination & initialize the ```FormDef```, ```FormEntryModel```, ```FormEntryController``` class objects to handle all form related functionalities.


## 5. ODK Consumer Reactive(ODK Transformer) Methods

#### 1. onMessage

It listens to the transformer kafka topic. Once a messages has been published to the kafka topic, it consumes the message, transform the message to get the next reply for user & send it to the next kafka topic.


#### 2. transform

It takes in the xMessage and returns a next XMessage to be send to user. It uses the MenuManager class to fetch the relavant data like xpath, next question etc. & validate the answer from user.


#### 3. updateQuestionAndAssessment

It fetches the previous question from database. If it does not exists in the database, it will first save the question then save the assessment data against the question.


#### 4. saveAssessmentData

It saves the assessment data in the database and generate a telemetry event to be send to the telemetry topic. Any service can then consume this telemetry event to process & generate reports for the assessment.  

#### 5. decodeXMessage

It saves the gupshup message entity & gupshup state entity with properties user phone, message, xpath, xml etc. It will return a XMessage with next message properties.


# Creating a new transformer

# User Segment

# Adapters Overview and how to add a new adapter

## 1. Conversation Channel Adapters

Adapters convert information provided by channels (SMS, Whatsapp) for each specific provider to xMessages and vise versa. Adapters are gateway to the external services and resposible to recieving user response and sending response to users. Thus the two major functions of Adapters are

- Convert API/webhook data from channel (and provider) to xMessages
- Convert xMessages back to API/webhook data format for the specific channel(and provider)

A simplified diagram of what adapters do is shown below. ![](https://samagra-development.github.io/docs/img/adapter.jpg)

## 2. How to setup adapter

Setup the adapter repository to start working on it. Follow the steps given to add a new adapter & test it. 

1. Fork the below repository & clone it.

	```https://github.com/samagra-comms/adapter/```
2. Checkout to the recent branch.
3. Open the project in any IDE spring/intellij.
4. Follow [point 3](#3-creating-your-own-adapters) to create a new adapter.
5. Write test cases for inbound method ```convertMessageToXMsg``` & outbound method ```processOutBoundMessageF```.
6. A simple example to test the send text message is given in the [file](https://github.com/samagra-comms/adapter/blob/release-4.8.0/src/test/java/com/uci/adapter/netcore/whatsapp/NetcoreServiceTest.java). 
## 3. Creating your own Adapters
The adapter and the inbound service are linked together as shown in the figure below. ![](https://samagra-development.github.io/docs/img/adapter-internal.jpg)
Similarly the adapter and the outbound service are linked it the following fashion. ![](https://samagra-development.github.io/docs/img/outbound.jpeg)
All adapters are named as `<ProviderName><ChannelName>Adapter`; for example GupshupWhatsappAdapter. Adapters should extend `AbstractProvider` and implement `IProvider`. Thus, it needs to implement the following methods:
- ```public Mono<XMessage> convertMessageToXMsg(Object msg) // Converts API response object to XMessage ```
- ```public Mono<XMessage> processOutBoundMessageF(XMessage nextMsg) //Converts XMessage object to API response and call it.```
These methods are called by `inbound` and `outbound` services internally to process the incoming and outgoing messages.
All adapters with the above implementation will be valid. An example adapter can be found [here](https://github.com/samagra-comms/adapter/blob/release-4.8.0/src/main/java/com/uci/adapter/netcore/whatsapp/NetcoreWhatsappAdapter.java).
#### 1. Incoming content
Currently we accepts text/media/location/quickReplyButton/list messages. It should be implemeted according to the network provider(Netcore/Gupshup) [documentation](https://wadocs.pepipost.com/webhooks/overview/incoming-message).
To enable these, we have to add them in ```convertMessageToXMsg``` and convert these according to the [xMessage spec](XMessage Spec.md)
#### 2. Outgoing Content
We can also send text/media/location/quickReplyButton/list messages to user. As we are using the ODK forms, we use bind::stylingTags, bind::caption to show the media file or to show the select choices as list/quick reply buttons. There are a few constraints which will be applied with the quickReplyButton/list content.
Below are the few styling tags we have allowed for now.
- image //To send image file
- audio //To send audio file
- video //To send video file
- document //To send document file
- buttonsForListItems //To send choices as quick reply buttons
- list //To send choices as list
To send this type of content to user, adapter should implement it according to the network provider(Netcore/Gupshup) [documentation](https://wadocs.pepipost.com/webhooks/overview/incoming-message).
## 4. List of Adapter Implementations
- Gupshup-Whatsapp
- Netcore-Whatsapp
## 5. FAQs
To be updated based on incoming feedback. Feel free to write into tech@samagragovernance.in in case you have questions, feedback or want to know more!
