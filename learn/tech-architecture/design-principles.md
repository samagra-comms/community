# Design Principles

1. Minimal specification
2. Utilize existing open-source services/infrastructure built by the community&#x20;
3. Create small non-opinionated components for extensibility (Manage diversity and complexity internally)&#x20;
4. Small independent services that can be easily maintained/replaced&#x20;
5. Interoperability using specifications&#x20;
6. Ensure privacy and security&#x20;
7. KISS
8. Population scale -&#x20;
   1. Asynchronous everywhere&#x20;
   2. Independently scalable small services&#x20;
   3. Databases that scale horizontally - Cassandra for persistence; Kafka pipeline for decoupling the Saga transactions
   4. Caching at all levels
   5. Deployment auto scalable based on Kafka queue - HPA (k8s)
9. **Plugin Based -** Adapters, Transformers and Users can all be injected by external providers. Allowing for un-opinionated implementation of specs (which are kept minimal) allowing for simpler, wider and faster adoption.
10. Federated Architecture - We only save configuration as part of the UCI and everything is just called from respective places.
11. **Minimal Microservices - A simple structure on how the microservices will interact with each other (specs) and how the information will be stored and processed.**
12. Data Federation - All data is secured using JWT and with extreme automation on the building of APIs using data specs.\
