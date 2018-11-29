# The five most important characteristics of cloud applications & platforms:

## Scalability:
 Horizontal & Auto Scale
## Distributed computing:
 Complex problems must be solved in shared manner across multiple instances
## API's: 
Your application and platform are easily accessible and extensible through public RESTful metadata and data API's
## Integration:
 Customers can easily integrate their data through user interfaces and API's
## Continuous deployment:
 Zero downtime and little to no scheduled maintenance

## Scalability

Infrastructure as a service vendors like Amazon AWS enable us to build platforms that can scale out in a matter of minutes, not days or months. To take advantage of this kind of functionality cloud platforms must consist of mostly stateless components. All of the components we are currently building for our aPaas can be run as single- or multi- instance. Any notion of state necessary to perform operations is shared using in-memory stores, either Cassandra or Redis. This enables new application servers to join the platform at any time and have fast, shared access to key data. When applications are shutdown, due to an auto-scale event, currently running tasks are either completed or returned to the Queue for other workers to process.

It’s important not to get lazy and build non performing software using auto scale mechanisms. You must make sure you are utilizing each instance in an optimal way and aren’t scaling because of poorly written code or algorithms. To ensure consistent behavior we enforce various performance metrics that each application must meet before we deem it suitable to scale out. Once those metrics are in an acceptable range we begin performance testing the application at scale.

Enabling true horizontal and auto-scaling does force us to think beyond shared in-memory stores and stateless application servers. There are many layers of the stack that will be impacted and the auto- scaling strategy must focus on the following areas:

Network infrastructure: load balancers, routing, and network address translating ( NAT-ing).
Instance capacity: Lack of availability of specific instance types or regions will force the triggering of scale events in different Amazon availability zones (AZ’s) or Regions.
Data stores: Sharding will be required because even the most modern data stores cannot handle the data volume anticipations of the future.
Queues: Your queues are only as good as your routing keys. Make sure that there are dedicated queues for specific events. Don’t be too granular or too broad.

## Distributed computing

Many developers build applications that have to vertically scale and require adding more memory or CPU to run. This pattern contradicts modern development best practices and isn’t suitable for cloud platforms. Whenever problems are too large to solve on a single machine, the application must be able to distribute the problem to a pool of workers and collaborate on the solution.

Developers should implement algorithms or computations as a set of of nodes in a graph. Nodes can be mapped into instance nodes, which are collections of nodes calculable (reduced) on a single instance. This mechanism requires building a very smart query planner that can quickly define nodes, instance nodes, and combine instance node results. The query planner must be able to decide within milli-seconds whether or not the calculation is too large for a single instance. And whether or not the time spent sending the problem to multiple instances outweighs the time and overhead to execute on a single instance. A great reference used by many developers in MapReduce.

Always assume that the application will fail. A good implementation of distributed computing requires the application to complete the work of separate instances that may have failed.

## API's

Many platforms lack clearly defined API’s. I always tell my team, anyone should be able to re-build our client side applications using our REST API. If you can’t publish your API publicly because it is too complex for other developers to understand, it will be a huge roadblock for your own team and the on boarding of new engineers.

Companies primarily focus on providing API’s to manipulate data within their application. However, it is very important to define great metadata API’s as well. A JSON schema is a great framework for this. Metadata API’s shouldn’t just define the structure of objects you are storing in your backend because they should enforce validation as well. When building applications and user interfaces, developers should be primarily driven by the metadata layer. This enables developers to quickly change validation patterns and behaviors for supporting new applications. The same rules and logic should apply to all applications consuming or generating the data, so that writing custom enforcement within applications can be avoided.

API’s generate valuable data points and metrics. It’s imperative that the application captures these and makes them available in the datastore. Developers need to implement an event capturing framework early on.

## Integration
Engineers are passionate about building applications and often times getting data in and out is an afterthought. In their defense, if it’s really to easy to integrate into your platform but your application isn’t great, integration won’t benefit you either, which implies there has to be a balance. Most applications require users and users need data within an application for it to be meaningful and work. Applications tend to focus on business users and not developers, which requires user-friendly interfaces for the business user, but don’t forget the engineers who need developer-friendly interfaces.



## Continuous deployment
I believe that disaster recovery is dead. The application should always be online and available, and it shouldn’t have to failover from one site to another. There shouldn’t be scheduled maintenance and hours of downtime for the users. When was the last time Facebook provided you with a maintenance window?
There are too many considerations for continuous deployment and zero down time to be covered here. eBay provides a great blog post on the topic here.


