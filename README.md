# Overview of Sunbird UCI

**Context**

The world runs on communication, notifications, and nudges. Notifications could be informative text, a reminder, or even a nudge. The use cases may range from a notification to a student on today's homework, to a notification of a new job posting to a job seeker, it can also be a notification to a farmer on the weather forecast or even a notification to an IAS officer about the weekly progress summary of the project. Also, the users are distributed across different communication channels like WhatsApp, SMS, Telegram, Email, In-App, etc. The goal of all the notifications is to capture the attention of the end-user and drive forward a piece of relevant useful information. Doing so may at times mean sending the same or similar conversation over more than one channel, e.g. Farmers receive the weather advisory over SMS, IVRS call, and even local radio.

**Problem Statement**

As a government or any service provider organization that wants to send the communication to the end users. The organization can send a one-way communication to users like an advisory, reminder, or nudge. Or can create a two-way communication channel like WhatsApp Bot or a chatbot on govt. portal. Each of these modules turns out to be a complex engineering effort, with a high touch point of engineers to make any modifications in the flow or add a new message or channel of communication.

**Vision**

UCI is created with a vision to make the conversation flow creation configurable to a level, that it can be done by program owners, without the intervention of any engineers at all. At the same time, it also envisions reducing the redundant coding required for enabling the same business logic for different channels.&#x20;

**How UCI Functions**

In short, UCI's architecture has created an abstraction layer between the Logic and the communication channels (WhatsApp, SMS, etc) and various providers (Like Gupshup, Twilio, etc.). UCI uses an adapter-based abstraction layer. An adapter has to be created for every channel+provider that is to be used with UCI. Once contributed, it is available for everyone in the community to leverage.

**Benefits of UCI**

Following are the key benefits of UCI: (not exhaustive)

* Provides an abstraction layer to safeguard PII data of users (Does not store or pass on)
* Makes new conversation logic creation configurable (possible to be done using a frontend)
* Considerable reduction of custom coding required for different channels

