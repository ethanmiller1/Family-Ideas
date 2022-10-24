# Core Stories
- [ ] Rancher Migration
- [ ] Cucumber POC / Reflective Framework
- [ ] Logging - MiNiFi
- [ ] Memory watcher
- [ ] Caching Apigee access token
- [ ] Logging PCI information (CC data)

# Problem Stories
- [ ] Logging - Sys Log Drain - Logstash Pods are still filling up the [var/lib/kubelet/pods](https://apm.aa.com/e/daa15b35-f63b-46fe-8465-781f95df871a/#newhosts/hostdetails;id=HOST-27834CE5A136F1AE;dd=DISK;tab=USAGE;gtf=-2h) directory and getting evicted
- [ ] Round Robin to a Bad DNS server
- [ ] Database source in config map (recorded DERP: [YouTube](https://youtu.be/UHYieC28vFs?t=628))
- [ ] MAX_READ_TIMEOUT after migration in our CCauth service - configurations didn't get carried over.
- [ ] Auto-scaling - ephemeral scaling, solved with Prometheus - Custom metrics
- [ ] [[Software Stories#Group Deposit Failures for More Than One Item|Group Deposit Failures for More Than One Item]]
- [ ] Memory leak - memory dump - new logger for each log.
- [ ] Quarkus bug with [[Software Stories#Quarkus Bug Calling Pagination with a negative index in JaxRs Repository implementation|Spring Data JAX-RS]]

## Secondary Problem stories
- [ ] Service Mesh - Port 80 vs 443 for service to service communication
- [ ] Persistent Volumes got deleted in stage, all of our apps went down

# Other Stories
- [ ] Brent Garrison -[[Software Stories#Brent Garrison PostConstruct Max Connections|Load Testing - Capacity, turned out was post construct]]
- [ ] Credit Card Encryption
- [ ] ADX update policies
- [ ] Extended Hold email 24 hours before it expires
- [ ] Lalit - $1 million a month [External RES Fee - Swagger change for the AGYCHG](https://rally1.rallydev.com/#/6722473906/iterationstatus?detail=/userstory/514528916780)
- [ ] Wiremock matching multiple mappings and ordering differently in the pipeline
- [ ] Memory kills - Increase non-heap memory
- [ ] Restoring artifacts to container registry
- [ ] Daily sign-in script for IBM Cloud using API key
- [ ] Pipelines downloading artifacts from public maven - setup custom settings.xml with our CR
- [ ] Cucumber Reporting only 1 Test
- [ ] Deployment order/one-time deployments
- [ ] Health port hidden in config map, previous version of spring
- [ ] Bilal config maps
- [ ] Second index for data partitions
- [ ] Kuma service mesh for secure service-to-service communication on port 443 (opposed to 80)

# Leadership stories
- [ ] Feedback: doing all the work for Jenifer, [[Software Stories#Mentorship|mentoring]], make too many jokes, team believes everything I say
- [ ] Deliver results: New Kiosk work, negotiating contracts, 

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

## x-hold - crash loopback in qa -- Cannot acquire connection from data source

x-hold - crash loopback in qa  --  Cannot acquire connection from data sourcejava.sql.SQLRecoverableException
ancillary-olay-ms  --  http://10.42.7.73:8080/health   Readiness probe failed   --   not in production
advredesign-deadletter-ms  --  [http://10.42.7.73:8080/health](http://10.42.7.73:8080/health)   Readiness probe failed   --   not in production

itkts - Invalid or corrupt jarfile /artifact.jar  --  because of powershell script that selected which jar  || SOLUTION: rebuild with task group changes

airextra-sws-ms  --  GOOD
emdmanager-ms  --  GOOD

1.  Changed to actuator/health
	-   BasePathAwareHandlerMapping - Did not find handler method for [/actuator/health]
2.  Changed to port 8911
	-   Readiness probe failed: Get "http://10.42.7.76:8911/health": context deadline exceeded (Client.Timeout exceeded while awaiting headers)
3.  Increased memory / cpu
4.  Increase delay period
5.  Download logs
	1.  Looking for request mappings in application context
	2.  EndpointHandlerMapping -  Mapped "{[/mappings || /mappings.json],methods=[GET],produces=[application/vnd.spring-boot.actuator.v1+json || application/json]}"
	3.  EndpointHandlerMapping -  Mapped "{[/health || /health.json],methods=[GET],produces=[application/vnd.spring-boot.actuator.v1+json || application/json]}"
	4.  request handler methods found on class
6.  to allow for resolving potential circular references

## Mentorship

At my first consulting job with a company called improving, my boss called me into his room one time for a 1-1, and the first words he said was, "Stop doing everything for Jennifer. You are like someone standing over her while she's bench pressing, holding all of the weight, robbing her of actually growing through the resistance of the weight." So he said that, and it really made sense to me, so going forward whenever I mentor people, I still explain a lot of thing, I still give a generous amount of help, but I'm always careful to make sure I give enough space for the people I'm mentoring to work everything out in their own mind. I ask them questions, and I wait for them to arrive a the right question to ask me before I answer it. And after making that change, fast forward to my next company at American Airlines, my squad is known for fast tracking young developers to the next level, mostly because of me, and my manager told me that they always send the adepts to our squad because we're for being really good at teaching. 

## Logging PCI information (CC data)

I was looking through our production logs one time, and I noticed that while we were masking PCI info in our APIs, any downstream service calls that used Sabre Web Services were showing PCI logs plain and clear. So I pinged our architect and said, "We're going to get hammered in our next PCI Audit if we don't fix this." Next day I presented the findings in a weekly meeting we have called Dev Forum and created an action plan. We created a library called [log4j2ext](https://dev.azure.com/AmericanAirlines/TicketingAndReceipts/_git/log4j2ext) that had a PCI masker, using the [Luhn Algorithm](https://www.geeksforgeeks.org/luhn-algorithm/) to mask Credit Card data intercepted by all of our logging.

The way that it works is any time we produce a log using Log4J, there's an **MDC context** (*Mapped Diagnostic Context*) per thread where we keep data about that transaction, like the TransactionId, the RecordLocator, ClientId, etc. These values are then picked up by our structured [logging pattern](https://dev.azure.com/AmericanAirlines/TicketingAndReceipts/_git/AOLMS?path=%2Fsrc%2Fmain%2Fresources%2Flog4j2.xml), the last of which is simply the log message, which simply has the logging key `%pci`.

``` xml
<properties>
	<!-- 
	  The "%cc" pattern converter is used to mask credit card numbers in log messages.
	  This converter is defined in the eTDS log4j2ext.jar library
	-->
	<property name="pattern">%d{ISO8601} %7sn %level{length=5} %.36X{ItktTranId} %.36X{ClientTranId} %X{INSTN} %X{CNTX} (%X{PNR}) (%K{response_status}) (%K{response_time}) (%K{called_service_name}) %c{1} -  %replace{%pci}{[\r\n]}{\u2028}%replace{%ex}{[\r\n]}{\u2028}%n</property>
	<property name="formatMsgNoLookups">true</property>
</properties>
```

Our log appenders use that pattern. There's an apache library called `log4j-core` that has an `LogEventPatternConverter` that you can extend, and then tag with `@ConverterKeys` , and give as arguments which keys in the logging pattern you want to filter through this converter.

``` java
@Plugin( name = PCIConverter.CONVERTER_NAME, category = PatternConverter.CATEGORY )  
// Eventually when we start masking other things that are not credit cards we might consider // changing this converter key to just use "pci" instead of now (current) cc.  
@ConverterKeys( { "cc",  
                  "pci" } )  
public class PCIConverter extends LogEventPatternConverter  
{
...
```


Simply override the `format()` method with the Masker we created, append it to the library's StringBuilder and boom, any Repo that uses this dependency now has all their messages PCI compliant.

``` java
public void format( final LogEvent logEvent,  
                    final StringBuilder toAppendTo )  
{  
   String text = logEvent.getMessage().getFormattedMessage();  
    
   // --- >>> CC number Masking <<< ---  
   String maskedMessage = this.ccMasker.mask( text );  
  
   // "toAppendTo" is never null as per log4j2 framework code.  
   toAppendTo.append( maskedMessage );
}
```

## System Log Drain

We were in the middle of a migration from Cloud Foundry to Rancher, everything was on track, and the eCaas team (Enterprise Communication as a Service) let us know extremely late, "Hey guys, by the way we're shutting down eCaas (where our logging infrastructure was) 2 months before we're shutting down ePaas (where out actual applications are)." So with the notice they gave us, we had 2 weeks to setup a temporary logging infrastructure literally only to last us for 2 months until our apps from ePaas were migrated. So we brainstormed, we already had a Logstash environment setup in IBM Cloud, so I took those deployments

1. modified them to fit into our Rancher / ePaas setup
2. submitted the necessary FARs for communication between our cluster and the ePaas
3. update the proxy in the Logstash http output to eventhub (because we were using a zscaler proxy for the communication)
4. and updated the System Log Drain in ePaas to point to our new Logstash service.

There were so many unforeseen issues I think I worked like 160 hours during those 2 weeks.

Logstash Pods are still filling up the [var/lib/kubelet/pods](https://apm.aa.com/e/daa15b35-f63b-46fe-8465-781f95df871a/#newhosts/hostdetails;id=HOST-27834CE5A136F1AE;dd=DISK;tab=USAGE;gtf=-2h) directory and getting evicted

![[Pasted image 20221023174756.png]]

Logstash has 3 components in the `pipeline.conf`:
1. Input (where all the logs from the pods that Filebeat is scanning gets dumped)
2. Filter (where you modify each log, maybe adding a namespace, parsing with a grok pattern), 
3. Output (where it sends it to some eventhub, in our case, ADX).

![](https://www.elastic.co/guide/en/logstash/current/static/images/basic_logstash_pipeline.png)

The problem happened that in the output, we were writing these massive amounts of logs not only to an http eventhub, but also to a standard output `stdout { codec => rubydebug }` which would write to `var/lib/kubelet/pods` on the host (the node those pods belong to), filling up that space allocated for it. Once the space fills up, all the pods that were running on that node would get evicted.

``` json
input
{
  tcp
  {
      port => 5044
      type => syslog
  }
  udp
  {
      port => 5044
      type => syslog
  }
}
filter
{
  if [type] == "syslog"
    {
      grok
      {
        match     =>
        {
          "message" => "(?<cf_logid>\\d{3}) <(?<cf_pri>[0-9]{1,5})\\>1 (?<cf_time>[^
          ]+) (?<cf_host>[^ ]+) (?<cf_msgid>[^ ]+) (?<cf_procid>[^ ]+) - - %{TIMESTAMP_ISO8601:app_timestamp}
          (?<cf_offset>[\\d\\s]{1,99}) %{DATA:app_loglevel} %{DATA:app_tranid} %{DATA:app_client_tranid}
          %{DATA:app_servername} %{DATA:app_version} \\(%{DATA:app_recordLoc}\\)(\\s+\\(%{DATA:response_status}\\))?(\\s+\\(%{DATA:response_time}\\))?(\\s+\\(%{DATA:called_service_name}\\))?
          %{DATA:app_className} - %{GREEDYDATA:app_message}"
        }
      }
      mutate
      {        
        convert => { "app_message" => "string"  }
        gsub    =>
        [
              "message",     'u2028', "\n",
              "app_message", 'u2028', "\n"
        ]
        add_field =>
        {  
              "app_env" => "$(namespace)"
        }
      }

    }
    if ("ProductFulfillmentResponse" in [app_message])
    {
        # Status mapping
    }
}

output
{  
  stdout { codec => rubydebug }
  http
  {
    url => "${EVENTHUB_URL}"
    content_type => "application/json"

    http_method => "post"
    format => "json"
    headers => {"Host"
            => "${EVENTHUB_HOST}"
            "Authorization" => "${SAS_TOKEN}"}
    proxy
    => "pzen.zsproxy.aa.com:9400"
  }
}
```

## Memory watcher

We were having an issue where a few times a week we kept getting these Dynatrace alerts about our Instant Ticketing app having long garbage collection.

First we enabled verbose logs in the JAVA_OPTIONS and uploaded the log file to [gceasy.io](https://gceasy.io/). And we say the Old Gen kept increasing steadily, we didn't see the nice shark tooth pattern that we wanted to see, so then we did some performance testing, did a thread dump, and analyzed the JVM memory breakdown with [YourKit Java Profiler](https://www.yourkit.com/java/profiler/features/) and [jvisualvm](https://visualvm.github.io/).

Ultimately we decided since iTKTS was a legacy app, and it was some third party dependency we couldn't identify, we implemented a workaround, a [memory watcher](https://dev.azure.com/AmericanAirlines/TicketingAndReceipts/_git/iTKTSCloud?path=/src/main/java/com/aa/etds/itkts/monitors/memory/MemoryCheck.java). 

``` java
@Component
public class MemoryCheck implements HealthIndicator
{

/**
* If the memory is low and the JVM needs to be restarted
* the following can be done:
*
* ( ( ConfigurableApplicationContext ) springContext ).close();
*
* For more information, see: https://docs.oracle.com/javase/8/docs/api/java/lang/management/MemoryUsage.html
*/
private void checkGCStatsAndShutdownIfNeeded()
{
  for( MemoryPoolMXBean mem : ManagementFactory.getMemoryPoolMXBeans() ) 
  {
	 if( "G1 Old Gen".equals( mem.getName() ) )
	 {
		MemoryUsage usage = mem.getCollectionUsage();

		// if last isn't the same as current, that means that a GC
		// event had occurred since our last check, so check mem 
		if( lastUsed != usage.getUsed() )
		{
		   lastUsed = usage.getUsed();
		   double ratio = ( double ) usage.getUsed() / ( double ) usage.getCommitted();

		   if( ratio >= properties.getMaxHeapThreshold() )
		   {
			  count++;
		   }
		   else
		   {
			  count = 0;
		   }
		   
		   // if we see at least N consecutive GC's nearly maxed out, 
		   // then we most likely have a memory leak
		   if( count >= properties.getMaxHeapCountThreshold() )
		   {
			  logger.error( "JVM is in a memory leak scenario. Will shutdown now." );
			  status = Health.down();
		   }
		}
	 }
  }
}
```

[OldGenerationMonitor.java](https://dev.azure.com/AmericanAirlines/TicketingAndReceipts/_git/iTKTSCloud/commit/7dff330f67fb69081137adfbd91a9b15954cbcdd?refName=refs/heads/master&path=/src/main/java/com/aa/etds/itkts/monitors/memory/OldGenerationMonitor.java)
``` java
@Component
public class OldGenerationMonitor implements ApplicationContextAware
{

@Scheduled( fixedRate = 10000 )
public void checkOldGenerationHeap()
{
  LoggingContextValues.updateMDC( getApplicationIndex(),
                                      getApplicationId(),
                                      "iTKTSMonitor",
                                      properties.getVersionName() );

      if( properties.getMonitorMemory() )
      {
         checkGCStatsAndShutdownIfNeeded();
      }

      LoggingContextValues.clearMDC();
   }
...

```

We did have an issue where the app was shutting down before Kuberenetes stopped sending traffic to the pod, so we set the grace period for 30 seconds to allow all the busy threads to finish before the app actually shut down.