# Tranformer 

Transformers transforms the previous xMessage from the user to one that needs to be sent next. It is a microservice that returns a new xMessage based on the previous user action, which will then be shown to the user. The XMessage will be pushed to the kafka topic, the outbound will listen to this topic to further process it.

