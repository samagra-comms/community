### Overview
It's a web client powered by [NextJs](https://nextjs.org/docs) ,[ChatUI](https://www.npmjs.com/package/samagra-chatui) and [Turborepo](https://turbo.build/repo) and using [UCI](https://github.com/samagra-comms) as backend.
It demonstrates how we can leverage UCI backend for communication.

## Home Page

![media](../../../media/01.png)

## Fetch bot List

It calls the UCI’s contextual api to get the bot list.

![media](../../../media/02.png)

Here is how the `allContextual` api works internally.

![media](../../../media/allContextual_flow_diagram.png)

## Socket Setup to start conversation

A socket connection is required to interact with the bot.

![media](../../../media/03.png)

Here **Authorization** and Channel header is a must, **deviceId** can be any unique identifier.Here for the socket, we have the following event,

![media](../../../media/04.png)

## Send Message
**“botRequest”** event is emitted for sending the message to the backend.

![media](../../../media/05.png)

Here is how the a single message flows through the entire UCI system.

![media](../../../media/bot_conversation_flow_diagram.png)

## Start Interacting with bot
To start the conversation with a bot you need to activate the bot first. For activating the bot we need to do the botRequest with the bot’s **“Starting Message”**.

![media](../../../media/06.png)

You can restart the conversation at any time by choosing the following option from the menu in the top right corner or sending aestric(*)  in botRequest.

![media](../../../media/07.png)

To go to the previous option you can choose the following option from the menu.

![media](../../../media/08.png)
## Download the media    
You can download the media by clicking on the download icon present in the bottom right corner of the media.
## Starred the message
 You can star the message by clicking on the start icon present in the bottom right corner of the message.

![media](../../../media/09.png)
## View Starred Messages
You can go to the starred page by clicking on the tab shown in the picture

![media](../../../media/10.png)

Select the bot to see the starred messages of that bot

![media](../../../media/11.png)

![media](../../../media/12.png)







