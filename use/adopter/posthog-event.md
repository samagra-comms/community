# Posthog Event

## Overview

PostHog is the all-in-one product analytics suite. PostHog provides everything product-led teams need in one place, including:

* Product Analytics
* Aggregate Dashboard
* Insights
* User lifecycle
* Event pipelines
* Export data

In UCI we are also using PostHog for creating aggregate dashboard by sending the events on each conversation. The events follow given [specification](https://github.com/sunbird-specs/Telemetry/blob/master/learn/specification.md).\


## Steps to use PostHog events

### 1. Create Posthog Account

First we would need to create a new account on [Posthog](https://app.posthog.com/signup) & get a project api key. The project api key will be available in [Project Settings](https://app.posthog.com/project/settings) of Posthog account.

### 2. Enable Posthog Event in UCI

To enable posthog events in UCI follow below steps.

1.  Add below environment variables for posthog url & api key. [Link to environments of UCI](../developer/development-environment/).\


    ```
        POSTHOG_TELEMETRY_APIKEY= // Api key
        POSTHOG_TELEMETRY_URL=https://app.posthog.com
    ```
2.  Add environment to enable posthog event.\


    ```
        POSTHOG_EVENT_ENABLED=TRUE
    ```
3. After the posthog events are enabled, UCI will send all conversations as events to posthog. we can generate aggregate dashboard with these events using insights, actions etc.&#x20;

### 3. Create insights in Posthog&#x20;

We can create insights like counts, graphs etc using posthog insights. Use this [documentation](https://posthog.com/docs) to better understand the insights, funnels, life-cycle etc that posthog provides.
