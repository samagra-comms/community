# Release Notes - UCI V2

## Overview

This release note contains all the feature additions, optimizations as well as changes that have been incorporated into the UCI ecosystem since the V1 version of UCI.

## New Features

- [Extend /health endpoints to make them async and detailed](https://github.com/samagra-comms/uci-apis/issues/54): Health endpoints now provide a more detailed result including all the dependent services.
- [Making query URL paginated](https://github.com/samagra-comms/broadcast-transformer/issues/1): The notification process now fans out in chunks instead of one at a time. This enables the UCI system to perform better and faster. A bigger page size means faster performance but a more CPU and RAM intensive process.
- [In bot details API, add icon image URL parameter](https://github.com/samagra-comms/uci-apis/issues/56): Conversation bots now support adding a display icon during the creation of bots.
- [Media upload API](https://github.com/samagra-comms/uci-apis/issues/55): Admin panel now supports uploading media files while creating a bot directly from the admin panel.
- [Add headers to query url being sent with broadcast conversation logic](https://github.com/samagra-comms/uci-admin/issues/15): Segment URL queries now pass an auth token to retrieve users.
- [Add a checkbox to create broadcast bot](https://github.com/samagra-comms/uci-admin/issues/14): Admin panel now supports a checkbox to create a broadcast bot along with a conversation bot.
- [Selected option highlighted](https://github.com/samagra-comms/uci-web-channel/issues/92): Web-Channel now highlights the previous options selected by users.
- [ [C4GT] Add Audio support to web channel](https://github.com/samagra-comms/uci-web-channel/issues/98): Web-Channel now supports audio messages.
- [Add Bot Icon and Media caption to the chatbot](https://github.com/samagra-comms/uci-web-channel/issues/111): Web-Channel now supports adding a caption to media.
- [Bulk Data Insertion in Fusion Auth](https://github.com/samagra-comms/uci-apis/issues/99): FusionAuth service now supports bulk insertion.
- [Create a bot with status as disabled by default](https://github.com/samagra-comms/uci-apis/issues/121): Disabled bots now return `ServiceUnavailableException` when triggered.
- [Return bot list in sorted order by name in allContextual endpoint](https://github.com/samagra-comms/uci-apis/issues/120): `allContextual` API now returns bots in sorted order.
- [Return purpose and description in botDetails API](https://github.com/samagra-comms/uci-apis/issues/119): Update API now allows updation of fields such as `purpose`, `description`, `endDate`.
- [Cassandra Server Configuration Pick from Docker-Compose](https://github.com/samagra-comms/inbound/issues/70): Docker Compose has been updated for Cassandra so that it picks up env from docker-compose file.
- [Add New Column in Cassandra for Notification Failure Reason](https://github.com/samagra-comms/dao/issues/17): Cassandra `xmessage` table now contains additional column to store notification failure.
- [Dockerize the web-channel to onboard to the one script deployment plan](https://github.com/samagra-comms/uci-web-channel/issues/123): UCI Web-Channel has now been dockerized.
- [Unify specs across UCI APIs](https://github.com/samagra-comms/uci-apis/issues/58): `/admin/search/internal` endpoint has been removed and only `/admin/search` is supported.
- [Add order by parameter in search API](https://github.com/samagra-comms/uci-apis/issues/174): Search API now also supports sorting by field.
- [Integrate Edit Bot in Admin Panel](https://github.com/samagra-comms/uci-admin/issues/42): Admin Dashboard now supports editing a bot.
- [Add loader to conversation-NL App](https://github.com/samagra-comms/uci-web-channel/issues/128): Web Channel now shows a loader until bot is loaded.
- [Handling of expired bots](https://github.com/samagra-comms/uci-apis/issues/163): Expired bots now show an expired message in web channel.
- [Broadcast Direct Message APIs](https://github.com/samagra-comms/outbound/issues/52): Direct message APIs can now send messages in broadcast mode.
- [Add endpoint to get bot report data](https://github.com/samagra-comms/uci-apis/issues/206): Bot Service now supports endpoint to get bot report data for notification.
- [Setup monitoring for bot health](https://github.com/samagra-comms/uci-apis/issues/211): Endpoint added for bot health.
- [Provide endpoints for deletion of bots](https://github.com/samagra-comms/uci-apis/issues/223): UCI-API now supports an endpoint for the deletion of disabled bots.


## Bug Fixes

- [Some tests are failing on stable branch (outbound)](https://github.com/samagra-comms/outbound/issues/28): Removed obsolete tests from outbound service.
- [Some test cases failing on stable branch (Broadcast transformer)](https://github.com/samagra-comms/broadcast-transformer/issues/4): Removed obsolete tests from Broadcast service.
- [Some tests are failing in stable branch (utils)](https://github.com/samagra-comms/utils/issues/42): Fixed failing tests in the utils package.
- [Some test cases failing on stable branch (inbound)](https://github.com/samagra-comms/inbound/issues/43): Fixed failing tests in inbound service.
- [Some tests are failing on stable branch (orchestrator)](https://github.com/samagra-comms/orchestrator/issues/35): Removed obsolete tests from Orchestrator service.
- [Some tests are failing on stable branch (message-rosa)](https://github.com/samagra-comms/message-rosa/issues/18): Fixed failing tests on message-rosa package.
- [Some tests are failing on stable branch (transformer)](https://github.com/samagra-comms/transformer/issues/66): Fixed failing tests on transformer
- [Fix {FileCDN}/getSignedUrl endpoints to accept fileName](https://github.com/samagra-comms/inbound/issues/51): FileCDN endpoints have been fixed to accept filename as a query parameter instead of static querying.
- [ [Bug] - Getting Exception Related to Unmarshaller](https://github.com/samagra-comms/orchestrator/issues/42): Marshaller error while converting XMessages parallelly has been resolved.
- [ [Bug] - Getting Exception Related to Kafka](https://github.com/samagra-comms/orchestrator/issues/43): Null pointer exception on Kafka has been resolved.
- [ [Bug] Duplication of Kafka topics from Orchestrator](https://github.com/samagra-comms/orchestrator/issues/58): Duplicate message consumption from kafka in orchestrator service has been fixed.
- [Prevent same bot same starting message to be created](https://github.com/samagra-comms/uci-apis/issues/166): UCI-API now prevents bots with same name or starting message as that of an existing bot to be created.
- [Fix src/modules/service/service.service.spec.ts test](https://github.com/samagra-comms/uci-apis/issues/142): Service test cases fixed.
- [Increase Minio Signed URL Expiry Time for NL-APP](https://github.com/samagra-comms/inbound/issues/82): Minio urls for media do not expire now.
- [[Fix Test] src/modules/conversation-logic/conversation-logic.controller.spec.ts](https://github.com/samagra-comms/uci-apis/issues/134): Conversation Logic controller tests have been fixed.
- [Fix src/app.controller.spec.ts test](https://github.com/samagra-comms/uci-apis/issues/145): App controller tests have been fixed.
- [Invalidate cache on transaction layer on bot update](https://github.com/samagra-comms/uci-apis/issues/202): Upading a bot now invalidates cache on transaction layer.

## Performance Improvements/Enhancements

- [Remove manual instance creation of NetcoreWhatsappAdapter](https://github.com/samagra-comms/inbound/issues/49): Removes manual instance creation of NetcoreWhatsappAdapter and uses dependency injection instead.
- [Add retry to FusionAuth in updateUser](https://github.com/samagra-comms/uci-apis/issues/87): Added retry mechanism to FusionAuth to prevent the abrupt failing of requests.
- [Notification Pipeline](https://github.com/samagra-comms/outbound/issues/35): The notification pipeline has been revamped to process more than 2L notifications without crashing services.
- [ [Bug] - Cassandra Related Issues in Inbound](https://github.com/samagra-comms/inbound/issues/55): Cassandra load issue was resolved by optimizing the queries.
- [Add Cache in allContextual API](https://github.com/samagra-comms/uci-apis/issues/71): Adds caching to allContextual API to enhance response time.
- [Add logging to UCI-APIS](https://github.com/samagra-comms/uci-apis/issues/69): Service level logs have been added to all the endpoints along with the total response time of functions.
- [Reduce JVM memory usage](https://github.com/samagra-comms/docker-deploy/issues/71): Dockerfile of java services have been modified to cap total memory usage and to use Semeru JVM instead of openjdk-jvm.
- [Improve error logging in UCI-APIs](https://github.com/samagra-comms/uci-apis/issues/112): UCI-API service now returns proper error messages on upload failure.
- [Cache cassandra result in conversation history](https://github.com/samagra-comms/inbound/issues/75): User conversation history from Cassandra is now being cached to improve performance.
- [Remove old implementation of getLatestXMessage](https://github.com/samagra-comms/inbound/issues/74): `getLatestXMessage` query has been optimized to perform better.
- [vault dependency removal](https://github.com/samagra-comms/uci-apis/issues/85): Removed mittwald vault package dependency due to errors.
- [ [Bug] - Getting Error in Aggregate Postgres](https://github.com/samagra-comms/transformer/issues/84): Aggregate db postgres connection pool has been added to prevent `too many clients` error.
- [Create a separate Kafka topic for notifications in each service](https://github.com/samagra-comms/inbound/issues/85): Notifications and Conversations now use separate topics for Kafka.
- [Fix conversation flow in the setup by install.py](https://github.com/samagra-comms/docker-deploy/issues/87): UCI docker deploy now provides a single script setup which automatically configures a test bot and other dependencies.

## Dependencies

- [NL backend](https://github.com/Mission-Prerna/Nipun-Lakshya-App): NL backend systems that are responsible for managing users.


## Documentation
- [Document changes done to Outbound](https://github.com/samagra-comms/outbound/issues/22): Updated Firebase adapter docs to include a table explaining all the required fields in the request.
- [Modify Admin Console Manual](https://github.com/samagra-comms/community/issues/52): The UCI documentation now also includes a manual for the admin panel.
- [Added a table explaining all the different fields that are used while creating a new conversation logic](https://github.com/samagra-comms/community/pull/28): Documentation has been updated to explain all the relevant query fields in conversation creation API.
- [Create flow diagram of message flow in UCI on Draw.io](https://github.com/samagra-comms/uci-apis/issues/150): Documentation now consists of Draw.io diagrams of different flows.
- [ [Docs] Update documentation of bot history api to match v2](https://github.com/samagra-comms/community/issues/37): Documentation for bot history api has been updated for v2.
- [ [Docs] Update documentation of bot setup api to match v2](https://github.com/samagra-comms/community/issues/36): Bot setup api has been updated in documentation to match v2.
- [ [Docs] Update Setting up first conversation according to v2](https://github.com/samagra-comms/community/issues/34): Documentation updated for setting up first conversation.
- [ [Doc] - Documentation for Database Schema](https://github.com/samagra-comms/dao/issues/18): Documentation added for database schema.
- [Update UCI Setup Docs](https://github.com/samagra-comms/docker-deploy/issues/91): Documentation has been updated with the latest setup guide which provides a more streamlined and hassle free installation process.

## Breaking Changes
- Old Admin Console is not compatible with V2 version of UCI.
- V2 version of UCI depends on NL backend and will not work without it.
- Gupshup provider has been removed from the pipeline.

## Contributors

- [geeky-abhishek](https://github.com/geeky-abhishek): Abhishek Kumar Pal
- [choxx](https://github.com/choxx): Abhishek Mishra
- [RyanWalker277](https://github.com/RyanWalker277): Anvansh Singh
- [devAyushDubey](https://github.com/devAyushDubey): Ayush Dubey
- [ChakshuGautam](https://github.com/ChakshuGautam): Chakshu Gautam
- [chinmoy12c](https://github.com/chinmoy12c): Chinmoy Chakraborty
- [karntrehan](https://github.com/karntrehan): Karan Trehan
- [pankajjangid05](https://github.com/pankajjangid05): Pankaj Jangid
- [tushar5526](https://github.com/tushar5526): Tushar Gupta
- [1010varun](https://github.com/1010varun): Varun Agarwal
- [yuvrajsab](https://github.com/yuvrajsab): Yuvraj Sablania

## Feedback

We welcome your feedback! If you encounter any issues or have suggestions for improvements, please [submit an issue](https://github.com/samagra-comms/community/issues) on our issue tracker.

Thank you for using UCI!
