# Design Principles

**Plugin Based -** Adapters, Transformers and Users can all be injected by external providers. Allowing for un-opinionated implementation of specs (which are kept minimal) allowing for simpler, wider and faster adoption.

**Minimal Micro-services -** A simple structure on how the micro-services will interact with each other (specs) and how the information will be stored and processed.

![](https://lh3.googleusercontent.com/OVWuTqk6shpts2Ar2fSX5Hui2\_MGH5n2O7J\_3JGtUF7LDs8iltWvW-FehjvpMhGE6L9XJkQeFWhThJW5P4KEiMESTIsdtD5Xmz-6fNGlbUCd9lXrBviDfhxa8It2sal-B9P0bhlr)

**Federated Architecture** - We only save configuration as part of the UCI and everything is just called from respective places.

![](https://lh3.googleusercontent.com/IuaVEYpOVGECXdtJlY1Zg46Ub0b9z9aiEXZAmhrcLzWg5zk3NDhZI9zrfOzdIx4kIBhQr6zpztN3mH5I08B5vu-2TrO4K2Jr5QppIebWdel36KQZ58RX1kF0kl2HqqMnQZs\_t2nz)

**Data Federation** - All data is secured using JWT and with extreme automation on the building of APIs using data specs.

![](https://lh6.googleusercontent.com/kpq39YFklZIt1EuxVHMwTzRuqoLF5PKTWF8GIJOdo7yDAt5MdWLuGHLUgHXM9FSFrn9jD75kHJ5dS2fzu2QHDBFyzzT6FZM7oPbpMmq8sUbHv2RaVsosGh8\_xMooy6W7lBQ5KBCC)

**Population scale** - Asynchronous everywhere with independently scalable small services. Databases that scale horizontally - Cassandra for persistence; Kafka pipeline for decoupling the Saga transactions. Caching at all levels. Deployment auto scalable based on Kafka queue - HPA (k8s).
