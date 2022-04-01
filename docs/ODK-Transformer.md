---
id: ODKTransformer
title: ODK Transformer
sidebar_label: ODK Transformer
---

## 1. Overview

A transformer is a stateless processing object that takes inputs and converts the input into a processed response. Transformers  may in turn call external services if needed. This transformer uses odk forms to get the input from user and output the next reply to be sent. ODK forms contains the questions and flow of the questions.   


## 2. Setup ODK Aggregate

[ODK Aggregate](https://docs.getodk.org/aggregate-intro/) is an open source Java application that stores, analyzes, and presents XForm survey data collected using ODK Collect. It supports a wide range of data types, and is designed to work well in any hosting environment.

With Aggregate, your team can:

- Host blank XForms used by ODK Collect or other OpenRosa clients
- Store and manage XForm submission data
- Visualize collected data using maps and simple graphs
- Export and publish data in a variety of formats

Aggregate can be hosted on cloud providers such as DigitalOcean, and Amazon Web Services, or your own local or cloud server. There's also a pre-configured virtual machine image that is ready to deploy on any computer.

Please check this link to find [how to set up ODK Aggregate?](https://docs.getodk.org/aggregate-setup/)

Refer this link to find steps to [use ODK.](https://docs.getodk.org/aggregate-use/)


## 3. How it works:

It saves all the question & assessment data in db and fetch the same when needed. This transformer uses MenuManager class to handle all ODK related processes. 

MenuManager uses [javarosa](https://mvnrepository.com/artifact/org.getodk/javarosa/3.3.0) library. It loads the odk form, fetches the next question based on previous xpath or no path, validate the answer to current question & generate the error messages if any.

A form is first loaded to the ```FECWrapper``` class. It gives us the ```FormEntryController``` class object to handle ODK related functionlity. Some of the main methods we use to fetch the relavent data using this class are given below.

1. Get current form index 

	```java
	FormController.getModel().getFormIndex()
	``` 

2. Set the current index to previous event of the form 

	```java
	FormController.stepToPreviousEvent()
	``` 

3. Set the current index to next event of the form 
	
	```java
	FormController.stepToNextEvent()
	```

4. TreeReference of the fully qualified element of the form 

	```java
	FormController.getModel().getFormIndex().getReference()
	```

5. Get the text for question at current index 

	```java
	FormController.getModel().getQuestionPrompt().getQuestionText()
	```

6. Get the type for question (Eg. select/input/audio/image etc.) 

	```java 
	FormController.getModel().getQuestionPrompt().getControlType()
	```


FormEntryController has different event types based on the ODK form, from which some are listed below.

1. Represents a question: ```EVENT_QUESTION``` 
2. Represents a question group: ```EVENT_GROUP``` 
3. Represents a repeat question: ```EVENT_REPEAT```


## 4. MenuManager Methods

#### 1. start

This method loads the odk form, process the current answer, identify the next question/reponse and returns a ```ServiceResponse``` which includes a few properties that will be used by the ODK Transformer.

1. Current xpath
2. XMessage payload for Next question with question text
3. Updated Instance XML
4. Form Version
5. Form ID
6. Next Question object


#### 2. addResponseToForm

It validates the current answer against the type of question (input/select choice/media etc.) & returns a status for the same.


#### 3. getIndexFromXPath

It gets the index from the give xpath.


#### 4. getXPath

It gets the xpath from the current form index.


#### 5. renderQuestion

Renders a question by cleaning its text & adding help text to it, if there is any.


#### 6. createView

Creates and returns a new view based on the event type passed in. It returns the XMessage payload with question text and its choices if the question is of select choice type.


#### 7. loadForm

It loads a ODK xml form from the given path. It creates form defination & initialize the ```FormDef```, ```FormEntryModel```, ```FormEntryController``` class objects to handle all form related functionalities.


## 5. ODK Consumer Reactive(ODK Transformer) Methods

#### 1. onMessage

It listens to the transformer kafka topic. Once a messages has been published to the kafka topic, it consumes the message, transform the message to get the next reply for user & send it to the next kafka topic.


#### 2. transform

It takes in the xMessage and returns a next XMessage to be send to user. It uses the MenuManager class to fetch the relavant data like xpath, next question etc. & validate the answer from user.


#### 3. updateQuestionAndAssessment

It fetches the previous question from database. If it does not exists in the database, it will first save the question then save the assessment data against the question.


#### 4. saveAssessmentData

It saves the assessment data in the database and generate a telemetry event to be send to the telemetry topic. Any service can then consume this telemetry event to process & generate reports for the assessment.  

#### 5. decodeXMessage

It saves the gupshup message entity & gupshup state entity with properties user phone, message, xpath, xml etc. It will return a XMessage with next message properties.
