## Architecting the Cloud - Michael J Kavis

- The five characteristics of cloud computing are network access, elasticity, resource pooling, measured service and on-demand self-service
- Cloud bursting ??

#### Cloud computing worst practices

- Traditional software - tightly coupled architecture: because software cannot function properly if it is separated from its physical environment. Cloud computing requires a loosely coupled architecture.
- Vertical scaling is accomplihsed by increasing the existing hardware either by adding more CPUs, memory, or disk space to the existing infrastructure or by replacing the existing infra with bigger and more powerful hardware.
- Horizontal scaling is accomplished by adding additional infra that runs in conjunction with the existing infra. This is known as scaling out. It is often donw at multiple layers of architecture.
- A stateless servuce is a service that is unaware of any information from previous requests or responses and is only aware of information during the duration of the time that the service is processing a given request.
- A best practice for companies leveraging SaaS or PaaS database technologies is to ensure that they have access to the data outside of the service provider, whether it is snapshots of database backups, a daily extract, or some method of storing recoverable data independent of the service and the provider.
- Netflix's Chaos Monkey - job is to break things in production to test real-time recovery of the overall platform
- The secret to well-architected cloud services is to fully understand and embrace the concepts of RESTful services. Many companies claim that they have service-oriented architectures (SOAs) but many implementations of Representational State Transfer (REST)-based SOA are nothing more than just a bunch of web services(JABOWS).

#### It starts with Architecture

- It is critical that all architecture decisions are made with business goals in mind first and foremost.

#### Choosing the Right Cloud Service Model

- point-of-sale (POS) systems
- We invested a lot of time and money in architecting a fail-over solution that included master-slave and cross-zone redundancy
- Cloud Foundry and OpenShift - open source projects that can be deployed on any infra
- Most PaaS vendors throttle an individual user's bandwidth to protect against network collisions and congestion
- IaaS requires the developers to managge memory, configure database servers and application servers to maximize throughput, etc
- Another reason for leveraging IaaS over PaaS is related to mitigating risks of downtime.
- Public cloud solution will have to include redundancy across virtual data centers.

#### The key to the Cloud

- A key best practice to writing loosely coupled software in the cloud is to store the application state on the client instead of the server, thus breaking the dependencies between the hardware and the software.
- Resources and representations must be loosely coupled
- By leveraging hypermedia as the engine of application state (HATEOAS), the application state is represented by a series of links - uniform resource identifiers or URLs - on the client side, much like following the site map of a website by following the URLs. When a resource (i.e. server or connection) fails, the resource that resumes working on the services starts with the URI of the failed resource (the application state) and resumes processing.
- A node failing is similar to shutting your car off for lunch and another node picking up where the failed node left off is similar to restarting the car and the GPS.
- Cloud architectures rely on Basically Available, Soft Sate, Eventually Consistent (BASE) transactions.
- Cloud based architectures require partition tolerance, meaning if one instance of a compute resource cannot complete the task, another instance is called on to finish the job.
- Cloud : rapid elasticity and resource pooling
- Building stateless, loosely coupled, RESTful services is the secret to thriving in this highly available, eventually consistent world.

#### Auditing in the Cloud

- Some well known regulations
  - ISO 27001
  - HIPAA
  - PCI DSS
  - SOX (Financial)

#### Data considerations in the Cloud

- Separate databases into read-only and write-only nodes
- EBS volumes are local disk systems that lack the reliability and redundancy of S3 but perform faster.
- One common strategy is to perform backups on a slave database so that the application performance is not impacted.
- The total isolation approach is commonly used when a tenant has enormous amounts of traffic.
- NoSQL is popular because: The increasing amount of data being stored and access to elastic cloud computing resources

#### Security Design in the Cloud

- Three key strategies for managing security in a cloud-based applications:
  - Centralization
  - Standardization
  - Automation
- Security is a nonfunctional requirement
- Areas to focus
  - Protection
  - Detection
  - Prevention

#### SLA Management

- SLAs are a pledge from a service provider to a service consumer that specific performance metrics will be met, a certain level of security and privacy will be upheld, and if required, the provider has been certified for specific regulations

#### Monitoring Strategies

- Recommended book: Building Scalable Websites by Cal Henderson

