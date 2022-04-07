# Example Usage Scenario

This section provides an example of usage of this unified interface. Purpose is to illustrate a set of real life use cases and not enumerate all possible usages. It is expected that the user ecosystem will innovate and find more interesting usage scenarios for this simple and unified communication interface.

**Usage Scenario**

Class teacher of Geeta, a class 5 student from Govt. School, Gurgaon, Haryana has shared a number with her. She has asked her to send a WhatsApp message ‘Hi UCIBot’ on this number. She has been told to enter her details on the bot and take a quiz and oral test. Teacher wants her to submit the printout of her scorecard in the next class.

**Steps for Geeta:**

1. Geeta sends starting message to the bot&#x20;
2. Geeta confirms her identity&#x20;
3. Geeta selects the subject for quiz&#x20;
4. Geeta answers all the questions in her quiz&#x20;
5. Bot prompts for oral assessment&#x20;
6. Geeta follows the instruction and starts to read&#x20;
7. At the end of the quiz, Geeta gets a link to download her report&#x20;
8. Geeta takes a printout of the quiz report and submits it to her class teacher (can also share the downloaded report via WhatsApp with her class teacher)

**Here is how it works:**

![](https://lh3.googleusercontent.com/-yK1nq1rAXT1ayOqOScKuGd7j7Rq4aUN2LDtGO7qqpB9hxUbGaNyHzt8inmkkCuhAZk300UeHOT1vMgPyQIALETwtHR76qGxo8LKbNsBYqchDoOgkrjECvZdM16VM1j-6qCccsji)

1. Student Initiates the conversation with bot&#x20;
2. User is mapped, if the user does not exist their profile is created&#x20;
3. Role based access level for the user is checked&#x20;
4. Check menu list mapped to user role is checked&#x20;
5. Menu list is updated (based on user)&#x20;
6. User is shown the dynamic menu&#x20;
7. User selects Hindi Quiz&#x20;
8. Orchestrator initiates the Quiz Transformer&#x20;
9. Quiz questions are updated&#x20;
10. Objective quiz is initiated&#x20;
11. Student submits Quiz answers for objective quiz&#x20;
12. Oral reading quiz is initiated&#x20;
13. Student’s audio is recorded and submitted&#x20;
14. Local language audio to is converted to text using external service&#x20;
15. Student results for audio test is updated&#x20;
16. Student text inputs are checked&#x20;
17. Student’s final result is updated&#x20;
18. Student’s report card (pdf format) is requested to be generated&#x20;
19. Orchestrator receives the report card from the PDF generator&#x20;
20. Additional content links to be shared with the student is requested&#x20;
21. Content links are updated for the message&#x20;
22. Student receives the report card with learning content links
