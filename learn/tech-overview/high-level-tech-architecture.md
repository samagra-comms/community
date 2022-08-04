# High-level Architecture Design

The block diagram below outlines various components that come together to form the Unified Communications Interface (UCI):

![Tech Architecture Diagram](<../../.gitbook/assets/UCI Architecture Updated.png>)

1. **Adapter**: An adapter translates between messages received from communication channels on specific providers and the internal XMessage format (followed by UCI). A new adapter is required for every new combination of communication channel and service provider (e.g. - WhatsApp + Gupshup; WhatsApp + NetCore; WhatsApp + Twilio). An adapter config references the communication channel, service provider and associated metadata such as a specific business account phone number.
2. **Transformer**: A transformer is a stateless processing object that takes inputs and converts the input into a processed output. Transformers may in turn call external services if needed.&#x20;
3. **Conversation Logic**: Conversation logic defines the overall flow for a specific conversation. A conversation logic object references a sequence of transformers that will be applied to arrive at the final response at a specific point in the conversation and the associated adapter config for this conversation logic.
4. **User Segments:** A bot can either be an open public bot or a closed targetted bot. In the latter case, a closed user segment has to be defined. User segment contains user data including a mechanism to fetch them from a federated user registry.
5. **Conversations:** A conversation orchestrates a conversation with a specific conversation logic assigned to a set of users. A bot object references user segment(s) and conversation logic(s).

Following is a summary of the relation between the different blocks of UCI :

`Adapter = Communication Channel + Service Provider`&#x20;

`User segment`&#x20;

`Transformer`&#x20;

`Conversation Logic = {Transformers} + Adapter`&#x20;

`Bot = {Conversation Logics} + {User segments}`

| Real World                                                     | UCI Platform                                                                                                                                            |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Set of end users in a state                                    | User segment 1 containing user registry uri, user ids, user metadata including phone numbers                                                            |
| Single end user phone number part of the state (eg. 938490834) | User phone number field in user segment object                                                                                                          |
| Whatsapp                                                       | Communication Channel referenced in the Adapter 1 config                                                                                                |
| Gupshup                                                        | Service provider referenced in the Adapter 1 config                                                                                                     |
| Business account phone number 908092348                        | Meta data for adapter 1, referenced in Adapter config                                                                                                   |
| If fail, then this message                                     | Transformer 1 referencing an XForm with pass/fail message logic                                                                                         |
| Translate message from English to Hindi                        | Transformer 2 referencing a translation microservice                                                                                                    |
| Given fail, send message via Whatsapp to student               | Conversation logic 1 = Transformer 1 + Transformer 2 + Adapter 1                                                                                        |
| Given fail, send same message via SMS to student               | Conversation logic 2 = Transformer 1 + Transformer 2 + Adapter 2 (where adapter 2 config references SMS communication channel and SMS service provider) |
| Lifecycle of conversation                                      | Conversation = Conversation logic 1 + Conversation logic 2 + User segment 1                                                                             |



****
