---
description: >-
  I want to use Sunbird UCI in my own project and I want to set up my own
  instance of Sunbird UCI
---

# Adopter - Install and use UCI

**Adoption guide - overview**

This page contains guides which walk through various aspects of UCI functionality. These documents are meant to give you a thorough understanding of the technology covered.

****[**Pre-requisites, Installation Setup, Post Setup**](pre-requisites-installation-setup-post-setup.md) ****&#x20;

This section lists everything you need to know about setting up the UCI on your own server. It starts with the infra pre-requisites, then details the Docker-based installation setup. At the end, the section covers the steps to test out various services that should be up. If you face any problem with one or more services, you can reach out to the community for help [here](https://github.com/samagra-comms/community/discussions).

****[**Setting up your very first conversation**](setting-up-your-very-first-conversation.md)****

After the setup is complete. The next step is to create a conversation and to start testing the UCI for an end to end custom logical conversation flow. There are two ways listed on this page:

1. Using the admin console to create the bot
2. Using the APIs in the sequence to create the bot\
   _The admin console internally uses the same APIs calls as mentioned in the second method_

After the bot is created you can actually test the bot on the UCI web channel. Web channel is an in-house created channel that can be used standalone or can be embedded in any existing application. It does not require any separate setup. It comes as a package with vanilla UCI setup.

****[**API Documentation**](api-documentation.md)****

This section lists all the relevant APIs required to use the UCI services. More APIs can be found in the Developer section of the documentation.

****[**Data Exhaust and Analytics**](data-exhaust-and-analytics.md)****

This section captures how the usage data for the bot can be extracted and used further for analytics purposes.&#x20;

****[**PostHog events**](posthog-event.md)****

UCI does not differentiate between any data storage and analytics tool. We are talking about PostHog as we found it to fit the bill for our needs. In this section, we share how you can setup Posthog, view events on PostHog and also create your relevant dashboards on it.

****
