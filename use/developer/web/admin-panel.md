## About UCI Admin Console

In order to start any conversation, we must first create a bot. These bots are powered by ODK. 
Repo: https://github.com/samagra-comms/uci-admin
The following steps are required to create a bot:-

#### Login To Admin Panel 
![media](../../../media/a1.png)

#### Once logged in you will be redirected to the dashboard, where you’ll see all the available bots.
![media](../../../media/a2.png)

#### Go to the Bot Create Page, by clicking on the add button.
![media](../../../media/a3.png)

#### Fill in the required fields.
![media](../../../media/a4.png)
 
  * Here, “Conversation Name” and the “Start Message”  should be unique for each bot.

  * “Create Broadcast bot” checkbox should be checked if you want to create a broadcast bot (Bot to send notifications to all the available users ).
 
  * Provide the segment Id (Unified Communication Interface | Messages)  

  * Provide the bot's Start and End Date.

  * Choose the bot Icon.

  * Once all valid details are provided you can move to the “Add Conversation Logic” Step

#### Add Conversation Logic
  Now, add the conversation logic (provide survey odk)  for the bot by clicking on “Add Logic”.
![media](../../../media/a6.png)

  * Fill in the valid logic details. 
   
![media](../../../media/a7.png)

#### Upload the ODK Form And Media (if there are any).

![media](../../../media/a8.png)

* If you are uploading a form with media,then you have to add the media name inside `{}` in the odk form where you want to have media.
![media](../../../media/a13.png)

{% hint style="info" %}
Once an ODK form is uploaded, the same form cannot be used again with the same form version.
{% endhint %}
 

#### Submit the form.
Once the form and media are successfully uploaded, click on the add button to submit the conversation logic.

![media](../../../media/a10.png)
##### [Sample Form](https://docs.google.com/spreadsheets/d/1SoOa4eRqOzNZ7VbiR8eEO0sjeZh7SRe2/edit?usp=sharing&ouid=100707492866343481921&rtpof=true&sd=true)

####  Add/View/Edit your conversation logic

![media](../../../media/a12.png)

#### Create a Bot,  click on submit button.

![media](../../../media/a9.png)

 Once successfully submitted, you’ll get a success screen and you can see your new bot in the dashboard’s bot list.

![media](../../../media/a11.png)

#### Demo
If you wish to access our staging admin console, kindly visit https://uci-admin-samagra.vercel.app/login. Kindly reachout to us for credentials. 



