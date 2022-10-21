- [ ] Brent Garrison -[[Software Stories#Brent Garrison PostConstruct Max Connections|Load Testing - Capacity, turned out was post construct]]
- [ ] Auto-scaling
- [ ] Cucumber POC
- [ ] Cucumber Reflective Framework
- [ ] Lalit - $1 million a month [External RES Fee - Swagger change for the AGYCHG](https://rally1.rallydev.com/#/6722473906/iterationstatus?detail=/userstory/514528916780)
- [ ] Rancher Migration
- [ ] Logging - MiNiFi
- [ ] Database source in config map (recorded DERP: [YouTube](https://youtu.be/UHYieC28vFs?t=628))
- [ ] [[Software Stories#Group Deposit Failures for More Than One Item|Group Deposit Failures for More Than One Item]]
- [ ] Prometheus - Custom metrics
- [ ] Service Mesh - Port 80 vs 443 for service to service communication
- [ ] Round Robin to a Bad DNS server
- [ ] Persistent Volumes got deleted in stage, all of our apps went down
- [ ] Memory leak - memory dump - new logger for each log.
- [ ] MAX_READ_TIMEOUT after migration in our CCauth service - configurations didn't get carried over.
- [ ] Wiremock matching multiple mappings and ordering differently in the pipeline
- [ ] Memory kills - Increase non-heap memory
- [ ] Restoring artifacts to container registry
- [ ] Daily sign-in script for IBM Cloud using API key
- [ ] Pipelines downloading artifacts from public maven - setup custom settings.xml with our CR
- [ ] Cucumber Reporting only 1 Test
- [ ] Extended Hold email 24 hours before it expires
- [ ] Quarkus bug with [[Software Stories#Quarkus Bug Calling Pagination with a negative index in JaxRs Repository implementation|Spring Data JAX-RS]]
- [ ] Deployment order/one-time deployments
- [ ] ADX update policies
- [ ] Health port hidden in config map, previous version of spring
- [ ] Memory watcher
- [ ] Bilal config maps
- [ ] Credit Card Encryption
- [ ] Second index for data partitions
- [ ] Caching Apigee access token

## Brent Garrison: PostConstruct Max Connections
We had an issue during **load testing** where we got severe **response time degradation** without any of our JVM metrics being impacted. My first thought was, "I've seen this before, probably the **capacity** on our **virtual services** are set too low and it's adding each request to a waiting queue." We logged into DevTest, opened up the virtualization, and nope. The capacity was set higher than the volume we were getting. We were pulling our hair out looking through Dynatrace, Kubernetes events, K9s for CPU and memory metrics, nothing. Finally after a whole morning of searching, we realized that we had MAX_CONNECTIONS set in our `application.yaml` file, but we weren't actually injecting it into the system properties, so we added a `@PostConstruct`  that increased the max connections to 50, and boom, problem solved.

``` java
@PostConstruct
public void setEnvProperties()
{
  String httpCons = env.getProperty( MAX_CONNECTIONS );
  if( null == httpCons || "".equals( httpCons ) )
  {
	 System.setProperty( MAX_CONNECTIONS,
						 params.getDefaultMaxPerRouteConnections() );
  }
  else
  {
	 System.setProperty( MAX_CONNECTIONS,
						 httpCons );
  }
}
```


## Group Deposit Failures for More Than One Item
I was enjoying my evening one day when I get a text from my buddy Gaspar, "Hey Ethan are you free to join a call? There's a SEV open and they gave us this PNR which failed in AOL." "Okay, sure. Let me sign in." First thing I noticed is we were getting a success response message, with a failed status. So my assumption was that we were mis-assigning the status. I checked the git history from the last time we deployed to production and found some code that compared the size of the number of responses to the size of the number of orderItems, and if they were not equal, we assigned a failed status. Well this works in every case except Group Deposit, because I may have one orderItem with multiple deposits, so we rolled back and fixed it the next day.
``` java
// If one AE fails, any subsequent AE is not processed. We still need to populate and return these so AOL does not get multiple null responses leading to a Duplicate key error when trying to put the responses into a map.  
if( response != null &&  
    response.getAeItemResponseList()  
            .size() != request.getOrderItems()  
                              .size() )  
{  
   if( response.getAeItemResponseList()  
               .get( 0 ) != null )  
   {  
      AirExtraOperationsResponse finalResponse = response;  
      aeRequest.getAeItemList()  
               .stream()  
               .filter( oi -> !oi.getItemId()  
                                 .equals( finalResponse.getAeItemResponseList()  
                                                       .get( 0 )  
                                                       .getItemId() ) )  
               .forEach( oi ->  
               {  
                  oi.setResponseStatus( "FAILED" );  
                  oi.setStatusCode( "FAILED" );  
                  finalResponse.addAeItemResponse( oi );  
               } );  
   }  
}
```
I wasn't on call, but I got a text from my friend, which turned out great because we have a program to recognize people's work by giving them Non-Stop Thanks points, and between my Manager, my Senior Manager, my Team Lead, and the guy that called me I got like $75 of non-stop thanks for honestly 30 minutes of my evening. And people wonder why I always pick up the phone.

DERP Presentation: [YouTube](https://youtu.be/kHf1uu_49NY?t=360)

## Quarkus Bug: Calling Pagination with a negative index in JaxRs @Repository implementation

### Failed to list the entities" when table is empty

I posted a question on [Stackoverflow](https://stackoverflow.com/questions/69340250/restdatapanacheexception-failed-to-list-the-entities-when-table-is-empty) and one of the [developers](https://www.linkedin.com/in/georgios-andrianakis/) asked me to [open a bug](https://github.com/quarkusio/quarkus/issues/20424) in the Quarkus framework repository on Github.

**Describe the bug** 
Using `@RepositoryRestResource` throws an exception when the table is empty.

**Expected behavior**  
Call to `/tasks` should return an empty list of tasks, similar to this response with an empty list of countries.

[![image](https://user-images.githubusercontent.com/55400451/134951841-f1f840c1-1ef3-43a8-93da-2848c2d44b0b.png)](https://user-images.githubusercontent.com/55400451/134951841-f1f840c1-1ef3-43a8-93da-2848c2d44b0b.png)
**Actual behavior**  
Call to `/tasks` triggers a `RestDataPanacheException` caused by: `java.lang.IllegalArgumentException: Page index must be >= 0 : -1`.

``` warn
2021-09-27 11:54:18,683 WARN  [io.qua.spr.dat.res.run.RestDataPanacheExceptionMapper] (executor-thread-0) Mapping an unhandled RestDataPanacheException: io.quarkus.rest.data.panache.RestDataPanacheException: Failed to list the entities
```

**To Reproduce**  
Steps to reproduce the behavior:

1.  `git clone https://github.com/ethanimproving/empty-list-defect.git`
2.  Create a MySQL Database called "scratch" and start the application. Ensure there is NO data in the task table.
3.  Send a GET request to `http://localhost:8080/tasks`

**Additional context**

The exception is handled in `quarkus-spring-data-rest`, so I suspect the issue is with that package.

[![image](https://user-images.githubusercontent.com/55400451/134952899-38d08a60-c5b7-4abe-a3d4-fb1152f09c04.png)](https://user-images.githubusercontent.com/55400451/134952899-38d08a60-c5b7-4abe-a3d4-fb1152f09c04.png)

Somehow the `list()` method of the JPA implementation of my TaskRepository (`TaskRepositoryJaxRs_47e783e09fc1ec6dc860f6e2c7cfb85a720cc58d`) is calling the `io.quarkus.panache.common` Pagination with a negative index. When my table is empty, my page index is -1 instead of 0.

[![image](https://user-images.githubusercontent.com/55400451/134954454-f7f3ee52-2406-4178-bfd6-b8dfdd945cae.png)](https://user-images.githubusercontent.com/55400451/134954454-f7f3ee52-2406-4178-bfd6-b8dfdd945cae.png)
The problem was that to offset the zero-based index they were summing the page index with negative one.

``` java
Integer.sum(pageCount, -1)
```

The [solution](https://github.com/quarkusio/quarkus/pull/22692) was fairly obvious, simply set a floor at zero and take the max. If it's less than zero, zero is the max.

``` java
Math.max(Integer.sum(pageCount,-1), 0)
```
