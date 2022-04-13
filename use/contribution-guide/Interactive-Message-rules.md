# Interactive Messaging

The Interactive Message Templates feature allows you to add buttons in
message templates that can be used with List and Quick Replies. Buttons will give the ability to develop interactive experiences with pre-set options for users.\
There are two types of buttons:

## 1. List Message

List messages provide a simpler and more consistent format than text-based lists for people to find and select\
List messages are a way to allow users to easily choose from up to 10 options.

### 1.1 Steps for creating List Message

To send a List Message follow the below steps while creating ODK form :

- **type** : `select_one <field_name>`\
In field name write name of field.

- **name** : `<field_name>`\
In name, write the same Field Name that we wrote in **type**.

- **label** : `<text>`\
In label, we can write text content that we want to attach with the message.

- **bind::stylingTags** : `list`
Bind tag indicates which type of message to be sent.

- For creating options as list items we do write the following properties in **_choices_** tab for each list item.

    - **list name** : `<field_name>`\
    In list name, we write the field name, the same we put in the _name_ field.
    - **name** : `<indexing>`\
    In name field, we write indexing of a list item
    - **label** : `<title>`\
    In label, we can write the title of a list item, which will be shown to the user.

### 1.2 Rules for creating List Message

- We can add up to 10 list items in a single list message. 
- In label of a list item we write up to 24 characters.
- In label, Alpha Numeric and Spaces are allowed.

### 1.3 Example for ODK form of List Message

- Survey Tab  
![Survey Tab](../../media/ListMessage_SurveyTab.png)
- Choices Tab  
![Choices Tab](../../media/ListMessage_CoicesTab.png)

### 1.4 Example of List Message Preview

![List Message](../../media/ListMessage_example.jpeg)

## 2. Quick Reply Button

These quick reply buttons will help improve the quality of conversations with users by prompting responses that can
reduce spelling errors and improve an automated experience.\
These buttons can be attached to text
messages and a maximum of 3 Buttons are allowed in one Message.

### 2.1 Steps for creating Quick Reply Button

To send Quick Reply Button follow the below steps while creating ODK form :

- **type** : `select_one <field_name>`\
In field name write name of field.

- **name** : `<field_name>`\
In name, write same Field Name that we wrote in **type**.

- **label** : `<text>`\
In label, we can write text content that we want to attach with a message.

- **bind::stylingTags** : `buttonsForListItems`
Bind tag indicates which type of message to be sent.

- For creating different Buttons in Quick replies we do write the following properties in **_choices_** tab for each Button.

    - **list name** : `<field_name>`\
    In list name we write the field name, the same we put in the _name_ field.
    - **name** : `<indexing>`\
    In name field, we write the indexing of a button.
    - **label** : `<title>`\
    In label, we can write the title of a button, which will be shown to the user.

### 2.2 Rules for creating Quick Reply Buttons

- We can add up to 3 Buttons in a single Quick Reply Message. 
- In label of a button we write up to 20 characters.
- In label, Alpha Numeric and Spaces are allowed.

### 2.3 Example for ODK form of Quick Reply Button

- Survey Tab  
![Survey Tab](../../media/QuickReplyButton_SurveyTab.png)
- Choices Tab  
![Choices Tab](../../media/QuickReplyButton_ChoicesTab.png)

### 2.4 Example of Quick Reply Button Preview

![Quick Reply Button](../../media/QuickReplyButton_Example.jpeg)
