# Utils

Utils communicate with the federation service via APIs. It holds all the utility/helper classes which will be used in other repositories.

## 1. Campaign Service

`Campaign` class contains methods for retrieving campaign data via different parameters :

- getCampaignFromID
- getCampaignFromNameTransformer
- getFirstFormByBotID
- getBotNameByBotID
- getCampaignFromNameESamwad


## 2. Bot Service

`BotService` class contains methods for retrieving bot and campaign data via different parameters :

- getBotFromStartingMessage
- getBotFromName
- getCampaignFromStartingMessage
- getCampaignFromName


## 3. Simple Kafka Producer

`SimpleProducer` class contains a method for sending data to Kafka topic.\
we use the `send` method from this class to send XMessages on different Kafka topics. 

## 4. Azure Blob Service

`AzureBlobService` class contains methods for uploading and retrieving files from **Azure Blob Storage**. 

- uploadFile
- uploadFileFromInputStream
- getFile
- getFileSignedUrl

## 5. User Service

`UserService` class contains methods for retrieving User data via different parameters :

- findByEmail
- findByPhone
- findByPhoneAndCampaign
- getUsersFromFederatedServers
- getUserByFullName
- findUsersForCampaign
- findUsersForGroup

