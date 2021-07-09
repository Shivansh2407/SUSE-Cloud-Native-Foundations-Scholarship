# Architecture Consideration for Cloud Native Applications

In this lesson, we will cover:

- Monoliths and Microservices
- Trade-offs for Monoliths and Microservices
- Practices for Application Development

## Monoliths and Microservices
![Monolith-Banner](https://www.pretius.com/wp-content/uploads/2018/12/monolithic-vs-microservices.png)

 -   Monolith: application design where all application tiers are managed as a single unit
 -   Microservice: application design where application tiers are managed as independent, smaller units

Monoliths and microservices have their place in the greater design ethos of APIs and are each appropriate for particular purposes. While the monolith delivers some simplicity and ease of development in the micro, the macro often benefits from a movement to microservices. The size of the application and the constituent elements changes the benefits derived from either solution. Thus, project scope bears ardent consideration when judging the two options for any codebase.

## Trade-offs for Monoliths and Microservices

- **Development Complexity**: Development complexity represents the effort required to deploy and manage an application. 
- **Scalability**: Scalability captures how an application is able to scales up and down, based on the incoming traffic. 
- **Time to Deploy**: Time to deploy encapsulates the build of a delivery pipeline that is used to ship features. 
- **Flexibility**: Flexibility implies the ability to adapt to new technologies and introduce new functionalities. 
- **Operational Cost**: Operational cost represents the cost of necessary resources to release a product. 
- **Reliability**: Reliability captures practices for an application to recover from failure and tools to monitor an application. 

Trade-Off | Monolith | Microservices
--- | --- | ---
Development Complexity | one programming language, one repository, enables sequential development| multiple programming languages, multiple repositories, enables concurrent development
Scalability | replication of the entire stack; hence it's heavy on resource consumption | replication of a single unit, providing on-demand consumption of resources 
Time to Deploy | one delivery pipeline that deploys the entire stack; more risk with each deployment leading to a lower velocity rate | multiple delivery pipelines that deploy separate units; less risk with each deployment leading to a higher feature development rate
Flexibility | low rate, since the entire application stack might need restructuring to incorporate new functionalities | high rate, since changing an independent unit is straightforward 
Operational Cost | low initial cost, since one code base and one pipeline should be managed. However, the cost increases exponentially when the application needs to operate at scale | high initial cost, since multiple repositories and pipelines require management. However, at scale, the cost remains proportional to the consumed resources at that point in time.
Reliability | in a failure scenario, the entire stack needs to be recovered. Also, the visibility into each functionality is low, since all the logs and metrics are aggregated together. | in a failure scenario, only the failed unit needs to be recovered. Also, there is high visibility into the logs and metrics for each unit. 

## Practices for Application Development

Practice | Description
--- | ---
Health Checks | Health checks are implemented to showcase the status of an application. Usually, health checks are represented by an HTTP endpoint such as `/healthz` or `/status`.
Metrics | Metrics are necessary to quantify the performance of the application. Usually, the collection of metrics are returned via an HTTP endpoint such as `/metrics`, which contains the internal metrics such as the number of active users, consumed CPU, network throughput, etc. 
Logs | Log aggregation provides valuable insights into what operations a service is performing at a point in time. It is the nucleus of any troubleshooting and debugging process. <br> Logging Levels include: <br> `DEBUG` - record fine-grained events of application processes <br> `INFO` - provide coarse-grained information about an operation <br> `WARN` - records a potential issue with the service <br> `ERROR` - notifies an error has been encountered, however, the application is still running <br> `FATAL` - represents a critical situation, when the application is not operational
Tracing | Tracing is capable of creating a full picture of how different services are invoked to fulfill a single request.
Resource Consumption | Resource consumption encapsulates the resources an application requires to be fully operational. This usually refers to the amount of CPU and memory that is consumed by an application during its execution.

**NOTE: If you find any topic missing or need elaborate explanation on a particular topic/heading, do open an issue or post it in the discussions!**

Hope this helps! Happy Coding :tada:
