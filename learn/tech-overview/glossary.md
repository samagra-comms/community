# Glossary

* **Communication channel**: Any channel/medium where the user comes and interacts with the systsem. Example. SMS, Whatsapp, Email, Slack, custom ChatBot etc.\

* **Service Provider**: Any authorized business that has the permission to sub-license the APIs of the communication channel is called a service provider. For example, Gupshup is a service provider for WhatsApp APIs, SMS APIs etc. Similarly there are other providers like Twilio, CDAC etc.\

* **Adapter**: An adapter translates between messages received from communication channels on specific providers and the internal XMessage format (followed by UCI). A new adapter is required for every new combination of communication channel and service provider (e.g. - WhatsApp + Gupshup; WhatsApp + NetCore; WhatsApp + Twilio). An adapter config references the communication channel, service provider and associated metadata such as specific business account phone number.\

* **Transformer**: A transformer is a stateless processing object that takes inputs and converts the input into a processed output. Transformers may in turn call external services if needed. \

* **Conversation Logic**: Conversation logic defines the overall flow for a specific conversation. A conversation logic object references a sequence of transformers that will be applied to arrive at the final response at a specific point in the conversation, and the associated adapter config for this conversation logic.\

* **User Segments :** A bot can either be an open public bot or a closed targetted bot. In the latter case, a closed user segment has to be defined. User segment contains user data including a mechanism to fetch them from a federated user registry.
* **Conversations :** A conversation orchestrates a conversation with a specific conversation logic assigned to a set of users. A bot object references user segment(s) and conversation logic(s).

****
