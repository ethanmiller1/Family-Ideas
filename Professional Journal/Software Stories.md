- [x] Brent Garrison - Load Testing - Capacity, turned out was post process
- [ ] Auto-scaling
- [ ] Cucumber POC
- [ ] Cucumber Reflective Framework
- [ ] Lalit - $1 million a month
- [ ] Rancher Migration
- [ ] Logging - MiNiFi
- [ ] Database source in config map (recorded DERP: [YouTube](https://youtu.be/UHYieC28vFs?t=628))
- [x] Group Deposit number of responses issue (recorded DERP)
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
- [ ] Quarkus bug with Spring Data JAX-RSd
- [ ] Deployment order/one-time deployments
- [ ] ADX update policies
- [ ] Health port hidden in config map, previous version of spring

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


## Group Deposit Failures for More Than One Items
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