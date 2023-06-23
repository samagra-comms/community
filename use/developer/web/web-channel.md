## About UCI :open_book:

The Unified Communications Interface (UCI) aims to democratize the use of different communication channels such as WhatsApp, Telegram, SMS, email and more for governance use cases through a standard configurable manner that is reusable and scalable across all governance use cases.

## Features :dart:

- Ability to connect to any communication channel through any service provider without doing custom changes in the core logic UCI.
- The UCI ecosystem is independent of external variables like communication channel and service provider powered by XMessage standard.
- Ability to have a configurable conversation logic for the bot
- Ability to connect to any database (local or federated) via services
- Ability to include value added services in the bot interaction flow through Microservices (Internal or External)
- Ability to create tools on top of UCI APIs to manage Bot configuration, conversations and visualization


### About UCI Web Channel
It's a web client powered by [NextJs](https://nextjs.org/docs) ,[ChatUI](https://www.npmjs.com/package/samagra-chatui) and [Turborepo](https://turbo.build/repo) and using [UCI](https://github.com/samagra-comms) as backend.
It demonstrates how we can leverage UCI backend for communication.



## Home Page
![The GitBook Logo](../../../media/01.png)




## Fetch bot List

 It calls the UCI’s contextual api to get the bot list.

![The GitBook Logo](../../../media/02.png)


## Socket Setup to start conversation

A socket connection is required to interact with the bot.

![The GitBook Logo](../../../media/03.png)

Here **Authorization** and Channel header is a must, **deviceId** can be any unique identifier.Here for the socket, we have the following event,

![The GitBook Logo](../../../media/04.png)


## Send Message
**“botRequest”** event is emitted for sending the message to the backend.


![The GitBook Logo](../../../media/05.png)

## Start Interacting with bot
To start the conversation with a bot you need to activate the bot first. For activating the bot we need to do the botRequest with the bot’s **“Starting Message”**.


![The GitBook Logo](../../../media/06.png)
You can restart the conversation at any time by choosing the following option from the menu in the top right corner or sending aestric(*)  in botRequest.


![The GitBook Logo](../../../media/07.png)

To go to the previous option you can choose the following option from the menu.


![The GitBook Logo](../../../media/08.png)
## Download the media    
You can download the media by clicking on the download icon present in the bottom right corner of the media.



## Starred the message
 You can star the message by clicking on the start icon present in the bottom right corner of the message.


![The GitBook Logo](../../../media/09.png)
## View Starred Messages
You can go to the starred page by clicking on the tab shown in the picture


![The GitBook Logo](../../../media/10.png)
Select the bot to see the starred messages of that bot


![The GitBook Logo](../../../media/11.png)

![The GitBook Logo](../../../media/12.png)






