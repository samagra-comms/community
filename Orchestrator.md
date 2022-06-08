# Orchestrator

The orchestrator authenticates & processes the user data from whom the message is received. The XMessage will then be pushed to the Kafka topic, the transformer will listen to this topic to further process it.

## 1. Authentication

Orchestrator receives User Data in the form of XMessage and authenticates the user via FusionAuth Service.\
FusionAuth help to manage the following data :

- authentication - who is this user?
- authorization - what can this user do?

## 2. Conversion between `from` and `to`

XMessage contains data about the **from** where the message is coming and **to** where it should be delivered.

In case when a message is coming from the user-end, it should come `from: User` and `to: Bot`, but in case of response from bot, it should come `from: Bot` and `to: User`.

this conversion between **to** and **from** is done in the orchestrator after authentication.

## 3. Push to Kafka topic

Now after _authentication_ and conversion between _from_ & _to_ Orchestrator push this XMessage to the Kafka topic and the transformer will listen to this topic to further process it.
