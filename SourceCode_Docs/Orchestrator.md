# Orchestrator

The orchestrator authenticates & processes the user data from whom the message is received. The XMessage will then be pushed to the Kafka topic, the transformer will listen to this topic to further process it.