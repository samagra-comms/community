# API Documentation

**I want to use UCI for my own use case how do I proceed?**

1. Setup UCI on your infra - [Steps](pre-requisites-installation-setup-post-setup.md)
2. Is the channel and provider you want to use is already supported in UCI?
   1. **If yes** -> Follow [these steps](../developer/api-documentation/bot-setup-apis.md#2.1-add-adapter) to add your adapter along with your own credentials for the platform
   2. **If No** -> Follow [these steps](../developer/contribution-guide/create-an-adapter.md#3.-creating-your-own-adapters) to contribute a new adapter and then follow the step above&#x20;
3. Follow these steps to [create your own bot](setting-up-your-very-first-conversation.md)



**In my use case, I want to send direct notifications to the users, I do not want to create a complete conversation flow. How Can I achieve this?**

1. Setup UCI on your infra - [Steps](pre-requisites-installation-setup-post-setup.md)
2. Is the channel and provider you want to use is already supported in UCI?
   1. **If yes** -> Follow [these steps](../developer/api-documentation/bot-setup-apis.md#2.1-add-adapter) to add your adapter along with your own credentials for the platform
   2. **If No** -> Follow [these steps](../developer/contribution-guide/create-an-adapter.md#3.-creating-your-own-adapters) to contribute a new adapter and then follow the step above
3. Follow the [direct Message APIs](../developer/api-documentation/direct-message-apis.md) to send the messages directly