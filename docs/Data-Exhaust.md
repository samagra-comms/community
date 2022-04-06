---
id: dataExhaust
title: Data Exhaust
sidebar_label: Data Exhaust
---

## 1. Overview

We can generate a data exhaust which is a list of conversations between the bot & user. When the bot & user converse, our service sends an event (AKA telemetry event) to a third party service which then uploads it to the blob storage.

When a messages is sent to the user & user replies to it, a telemetry event will be generated & will be sent to the third party service. We can then download a list of these events for a specific conversation for a time period using the apis mentioned in the document. 

The telemetry event sent is generated according the specification mentioned in the [link](https://github.com/sunbird-specs/Telemetry/blob/master/learn/specification.md). 

## 2. Data Exhaust Requests

We can generate a job for two types of exhaust requests.

#### 1. UCI Response Exhaust

This will provide a list of conversation done by any user, between the time period mentioned in the exhaust request. Each event consists of the user device id, question asked, question options if any, question reply from user, timestamp etc. A conversation event is basically a telemetry event that we send for each conversation message. A sample for the [uci response exhaust](../media/UCI-Response-Exhaust.csv)

#### 2. UCI Private Exhaust

This will provide a list of users that have conversed with the bot. It consists of user phone number(If consented by user), device id, username etc. The device id is the one that connects the response & private exhaust. To identify the phone number for the conversation done by the user, check the device id from the response exhaust, it should exists in the private exhaust. A sample for the [uci private exhaust](../media/UCI-Private-Exhaust.csv) 

## 3. Data Exhaust Apis

#### 1. Api to submit a job request for response/private exhaust

This api will submit a job request to fetch the response/ptivate exhaust for a specific conversation(bot) id based on the dataset config. The api will give a requestID in the response. The requestID & tag are used to fetch the job info.

1. URL: 
```
{{host}}/dataset/v1/request/submit
```

2. Method: ```POST```

3. Sample Request Body: 
```
	{
	  "id": "ekstep.analytics.job.request.submit",
	  "ver": "1.0",
	  "ts": "2022-02-15T10:07:00+05:30",
	  "params": {
	    "msgid": "06fb2660-88ba-11ec-bea0-578c52d1ac65"
	  },
	  "request": {
	        "dataset": "uci-response-exhaust", // dataset should be "uci-private-exhaust" or "uci-response-exhaust"
	        "tag": "7ed871f7-0d5b-4bb0-86a0-a00151352706", // random UUID
	        "requestedBy":"user_id",
	        "requestedChannel": "01309282781705830427", // channel id for the bot
	        "datasetConfig": {
	            "type": "uci-response-exhaust", // type should be "uci-private-exhaust" or "uci-response-exhaust"
	            "conversationId": "c63b6faa-11a9-4259-b941-11cd2f8446c6", // bot id
	            "startDate": "2022-03-01", // fetch the response data from date 
	            "endDate": "2022-04-01" // fetch the response data to date
	        },
	        "output_format":"csv" //output format
	    }
	}
``` 

4. Sample Response
```
{
    "id": "ekstep.analytics.dataset.request.submit",
    "ver": "1.0",
    "ts": "2022-04-06T09:32:00.079+00:00",
    "params": {
        "resmsgid": "d15de3c9-875d-4fbd-912c-4daa1cee0f1b",
        "status": "successful",
        "client_key": null
    },
    "responseCode": "OK",
    "result": {
        "attempts": 0,
        "lastUpdated": 1649237520065,
        "tag": "7ed871f7-0d5b-4bb0-86a0-a00151352706:01269878797503692810", // Tag to be used in job info api, but without the channel id
        "expiresAt": 1649239320077,
        "datasetConfig": {
            "type": "uci-response-exhaust",
            "conversationId": "c63b6faa-11a9-4259-b941-11cd2f8446c6",
            "startDate": "2022-03-01",
            "endDate": "2022-04-01"
        },
        "downloadUrls": [],
        "requestedBy": "user_id",
        "jobStats": {
            "dtJobSubmitted": 1649237520065,
            "dtJobCompleted": null,
            "executionTime": null
        },
        "status": "SUBMITTED",
        "dataset": "uci-response-exhaust",
        "requestId": "1E2A13DA30671FDBA10C418C9EF14A70", // Request id to be used in job info api
        "requestedChannel": "01269878797503692810"
    }
}
```

#### 2. Api to get job info of a job

This api is used to get the job info which includes the job status (Eg. Submitted, Success, Failed), download urls (if the job is successful), job request params etc. Tag & Request Id for the request will be used from the submitted job request.

The download url is a signed url which is valid for one day only.

1. URL
```
{{host}}/dataset/v1/request/read/{{tag(bot/conversation Id)}}?requestId={{requestId}}
```
```
Eg: {{host}}/dataset/v1/request/read/7ed871f7-0d5b-4bb0-86a0-a00151352703?requestId=1E2A13DA30671FDBA10C418C9EF14A70
```

2. Method: ```GET```

3. Sample Request Params
```
requestId=1E2A13DA30671FDBA10C418C9EF14A70
```

4. Sample Response
```
{
    "id": "ekstep.analytics.dataset.request.info",
    "ver": "1.0",
    "ts": "2022-04-06T09:36:21.181+00:00",
    "params": {
        "resmsgid": "f5c95bc2-f643-41f9-adb1-d566d03cf411",
        "status": "successful",
        "client_key": null
    },
    "responseCode": "OK",
    "result": {
        "attempts": 0,
        "lastUpdated": 1649165260152,
        "tag": "7ed871f7-0d5b-4bb0-86a0-a00151352703:01269878797503692810",
        "expiresAt": 1649239580993,
        "datasetConfig": {
            "type": "uci-response-exhaust",
            "conversationId": "c63b6faa-11a9-4259-b941-11cd2f8446c6",
            "startDate": "2022-03-01",
            "endDate": "2022-04-01"
        },
        "downloadUrls": [
            "https://sunbirddevprivate.blob.core.windows.net/reports/uci-response-exhaust/1E2A13DA30671FDBA10C418C9EF14A70/c63b6faa-11a9-4259-b941-11cd2f8446c6_response_20220405.csv?sv=2017-04-17&se=2022-04-06T10%3A06%3A21Z&sr=b&sp=r&sig=L5etefAbBA/2PUQHaqSvbDPbyC%2BPfftS2jkf8y64R6E%3D" // download url for the request exhaust 
        ],
        "requestedBy": "user_id",
        "jobStats": {
            "dtJobSubmitted": 1649165118347,
            "dtJobCompleted": 1649165260152,
            "executionTime": 6210
        },
        "status": "SUCCESS",
        "dataset": "uci-response-exhaust",
        "requestId": "1E2A13DA30671FDBA10C418C9EF14A70",
        "requestedChannel": "01269878797503692810",
        "statusMessage": ""
    }
}
```

#### 3. Api to get jobs list

This api is used to fetch the jobs list for a specific tag/bot.

1. URL
```
{{host}}/dataset/v1/request/list/{{tag(bot/conversation Id)}}?limit=5
```
```
Eg. {{host}}/dataset/v1/request/list/7ed871f7-0d5b-4bb0-86a0-a00151352703?limit=5
```

2. Method: ```GET```

3. Sample Response
```
{
    "id": "ekstep.analytics.dataset.request.list",
    "ver": "1.0",
    "ts": "2022-04-06T09:38:26.540+00:00",
    "params": {
        "resmsgid": "5aebb821-b821-4c66-a5da-1df3af84da65",
        "status": "successful",
        "client_key": null
    },
    "responseCode": "OK",
    "result": {
        "count": 1,
        "jobs": [
            {
                "requestId": "84B26F9012D6E2E53BFC422B245C6350",
                "tag": "7ed871f7-0d5b-4bb0-86a0-a00151352703:ORG_001",
                "dataset": "uci-response-exhaust",
                "requestedBy": "user_id",
                "requestedChannel": "ORG_001",
                "status": "SUCCESS",
                "lastUpdated": 1649165260152,
                "datasetConfig": {
                    "type": "uci-response-exhaust",
                    "conversationId": "d655cf03-1f6f-4510-acf6-d3f51b488a5e",
                    "startDate": "2022-03-01",
                    "endDate": "2022-04-01"
                },
                "attempts": 0,
                "jobStats": {
                    "dtJobSubmitted": 1649165118347,
                    "dtJobCompleted": 1649165260152,
                    "executionTime": 6210
                },
                "downloadUrls": [
                    "https://sunbirddevprivate.blob.core.windows.net/reports/uci-response-exhaust/84B26F9012D6E2E53BFC422B245C6350/d655cf03-1f6f-4510-acf6-d3f51b488a5e_response_20220405.csv?sv=2017-04-17&se=2022-04-06T10%3A08%3A26Z&sr=b&sp=r&sig=6N9Kz4P/t9wZ1sCn3mYdVWZ22IfyyqrUNp3dNyelpJ0%3D"
                ],
                "expiresAt": 1649239706533,
                "statusMessage": ""
            }
        ]
    }
}
```

Postman collection: [Link](../media/DataExhaust-APIs-postman_collection.json)

Environment: [Link](../media/Data Exhuast-postman_environment.json)

## 4. Execute Data Exhaust Job

The jobs will be executed via a cron job that runs daily at 02.00 AM. We should check the job info, the next day to check the job's current status. 

If we need an immediate result we can also run a job from jenkins AnalyticsReplayJobs service. Follow below step to execute the jobs.

1. Login to jenkins & Go to the AnalyticsReplayJobs service from the Deploy->Dev->DataPipeline services list or directly hit the [url](https://10.20.0.14/jenkins/job/Deploy/job/dev/job/DataPipeline/job/AnalyticsReplayJobs/).

2. Go to Build with Parameter.

3. Select ```replay-job``` from the ```job_type``` dropdown.

4. Select ```uci-response-exhaust```/```uci-private-exhaust``` from the ```job_id``` dropdown. 
![Screenshot](../media/exhaust-jenkins-job.png)

5. Leave the rest as it is & click build. It will run all the submitted jobs.

6. When its done, run the [job info api](#2-api-to-get-job-info-of-a-job) to fetch the job response. The job will have either the success/failed status. If the job is successful it will have a download url in the reponse. 
```
Eg. https://sunbirddevprivate.blob.core.windows.net/reports/uci-response-exhaust/84B26F9012D6E2E53BFC422B245C6350/d655cf03-1f6f-4510-acf6-d3f51b488a5e_response_20220405.csv?sv=2017-04-17&se=2022-04-06T10%3A08%3A26Z&sr=b&sp=r&sig=6N9Kz4P/t9wZ1sCn3mYdVWZ22IfyyqrUNp3dNyelpJ0%3D
```