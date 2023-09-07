## Overview


[UCI Admin](https://github.com/samagra-comms/uci-admin) is a web client powered by [NextJs](https://nextjs.org/docs) ,[ChatUI](https://www.npmjs.com/package/samagra-chatui) and [Turborepo](https://turbo.build/repo) and using [UCI](https://github.com/samagra-comms) as backend.
It demonstrates how we can manage the conversation bot.

In order to start any conversation, we must first create a bot. These bots are powered by ODK. 
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

#### Monitoring

Monitoring consists of all the various services along with the Overview section. The services include UCI-API, Inbound, Outbound, Orchestrator, Broadcast Transformer and Transformer. For each service, there are 3 types of visualizations provided namely Bar Chart, Pie Chart and Line Chart. One needs to select the date and type of visualization.

#### 1. Overview Section

This sections gives the high level details of the system such as the number of notifications received, number of odk login fail etc. It gives a high level detail of all the services present such as UCI-API, Inbound, Outbound, Orchestrator, Broadcast Transformer and Transformer. The user just needs to select the date from the dropdown and the data will be displayed if present.

![media](../../../media/m1.png)

#### Enter date in the date input which has an auto suggesting dropdown.

![media](../../../media/m2.png)

#### The data for the particular chosen date would be represented as follows:

![media](../../../media/m3.png)

#### 2. UCI-API

#### If a date was already chosen in any of the services section or overview section, then bar chart(by default) would be shown based on the data available

![media](../../../media/m4.png)

#### The screenshots below represent the different types of visualizations that are available:

Bar Graph

![media](../../../media/m4.png)

Pie chart

![media](../../../media/m5.png)

Line Graph

![media](../../../media/m6.png)

Similarly, we have the same functionality for all the services listed below:

![media](../../../media/m7.png)

#### 3. Logs

Fine grain logs are grepped from logs file for a particular date and time chosen by the user. The user can chose the start and end date along with the time to get the logs between that particular date and time. The user has to select the number of lines. After the selection of number of lines, let's say it is n, then n last lines of logs would be displayed. The user can toggle between the normal and error logs. The logs are displayed using ANSI encoding. One can filter the logs by typing any string they want to find. They can download the logs and the downloaded file will be saved in the local computer with the file name "Service_name+date_time" chosen.

#### Initially, by default the time and date will the present date and time.

![media](../../../media/m8.png)

#### One can select the date and time as per his/her requirement.

![media](../../../media/m9.png)

#### After the selection of date and time, it is necessary to select the number of lines.

![media](../../../media/m10.png)

#### There is a dropdown suggesting number of lines. One can choose from it or type the number of lines as per the requirement.

![media](../../../media/m11.png)

#### After selection of number of lines, the last "n" where n is number of lines logs are displayed for the particular date and time chosen.

#### The screenshots below represent the two types of logs that are available:

Normal logs

![media](../../../media/m12.png)

Error logs

![media](../../../media/m13.png)

#### The screenshots below represent the way the two types of logs can be filtered:

Normal logs

![media](../../../media/m14.png)

Error logs

![media](../../../media/m15.png)

#### One can click on the download icon and the logs will be downloaded in the following format:

![media](../../../media/m16.png)

#### Demo
If you wish to access our staging admin console, kindly visit https://uci-admin-samagra.vercel.app/login. Kindly reachout to us for credentials. 



