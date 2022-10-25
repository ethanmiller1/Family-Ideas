# Core Stories
- [ ] Rancher Migration
- [ ] Cucumber POC / Reflective Framework
- [ ] Logging - MiNiFi
- [ ] Memory watcher
- [ ] Caching Apigee access token
- [ ] Logging PCI information (CC data)
- [ ] Decupling Unit Tests, using Wiremock to move tests up to the API level
- [ ] Dynamic Routing Slip
- [ ] Performance Testing - fine tuning container memory / cores, find thresholds for auto-scaling
- [ ] Cucumber - Regression testing was a mess, long runtime, not versioned, passing around

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
- [ ] MFOP, Seats, MMX, Kiosk
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

First we enabled verbose logs in the JAVA_OPTIONS and uploaded the log file to [gceasy.io](https://gceasy.io/). And we saw the Old Gen kept increasing steadily, we didn't see the nice shark tooth pattern that we wanted to see, so then we did some performance testing, did a thread dump, and analyzed the JVM memory breakdown with [YourKit Java Profiler](https://www.yourkit.com/java/profiler/features/) and [jvisualvm](https://visualvm.github.io/).

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

## Incorrect Casing for Service Discovery
-   orderGroupResponse was invalid - this is a fake orderItemResponse 422
-   [DETTBP](https://dataexplorer.azure.com/clusters/private-orionadxp.eastus/databases/p-ticket-tktrcpts-applogs-adxdb?query=H4sIAAAAAAAAA12NPQvCMBCG9/6Ko1MKhYJ7HUQ3FdHu4ZocMdo24S4ogj/eIFXB9Xnejx71EJxofxUdOdiiaeAJ9zMxgU5+JEk4RlgCuqAWtiq+kskEtttgMAWGtoVyvem61aHMkSyIoX9ACpLYT07piCxkj+9SBSgmx/LhhUyCnMkr6vdXQwmgyho0xrjHkTKoZmJwGMieiG/e0J9jkhgmoc5/8LyxIxF0VL0ApT4lfO8AAAA=)
-   SessionManager.designatePrinter: HttpClientErrorException$NotFound: 404
-   applicationn.yaml: sessionManager: `http://sessionManager-ms/api/v1`
![[Pasted image 20221024161400.png]]
-   IKS: `sessionmanager-ms`
![[Pasted image 20221024161512.png]]
-   Every single call to session manager in AOL is failing.
-   Solution: use lowercase in service discover.

## JsonSubTypes

We have an `OrderFulfillmentRequest.java` that contains a list of OrderItems, and depending on the product, it may contain 1 of 20 or so subtypes that inherit from OrderItem. The problem is that the way we were generating the Client Stubs from our YAML contract, we had a List of POJOs, so our ObjectMappers didn't know how to serialize it.

![[Pasted image 20221024161849.png]]


Because of that, we had to turn the JSON into a HashMap, get the ItemType from that map, and then maintain a functional map that looks at the ItemType and maps it to the corresponding OrderItem. Now that was a bad design because it violates the open/close principle. Any new products we introduce, now we have to modify the functional map.
``` java
public Map< ItemTypeEnum, Function< LinkedHashMap< String, String >, AncillaryOrderItem > >  
       getMappingFunctionMap()  
{  
   var funMap = new EnumMap<>( ItemTypeEnum.class );  
  
   funMap.put( ItemTypeEnum.GROUP_DEPOSIT,  
               oim -> mapper.convertValue( oim, GroupDepositOrderItem.class ) );  
   funMap.put( ItemTypeEnum.PREFERRED_SEAT,  
               oim -> mapper.convertValue( oim, SeatOrderItem.class ) );  
   funMap.put( ItemTypeEnum.PAID_SEAT,  
               oim -> mapper.convertValue( oim, SeatOrderItem.class ) );   
   funMap.put( ItemTypeEnum.MISC_SVCS_FEE,  
               oim -> mapper.convertValue( oim, FeeOrderItem.class ) );  
   funMap.put( ItemTypeEnum.MILEAGE_REINSTATEMENT_FEE,  
               oim -> mapper.convertValue( oim, AncillaryOrderItem.class ) );  
   funMap.put( ItemTypeEnum.ADHOC_TRIPCREDIT,  
               oim -> mapper.convertValue( oim, AncillaryOrderItem.class ) );  
   funMap.put( ItemTypeEnum.MILEAGE_MULTIPLIER,  
               oim -> mapper.convertValue( oim, MileageMultiplierOrderItem.class ) );  
   funMap.put( ItemTypeEnum.INSTANT_UPSELL,  
               oim -> mapper.convertValue( oim, InstantUpsellOrderItem.class ) );  
   funMap.put( ItemTypeEnum.PREPAID_BAGS,  
               oim -> mapper.convertValue( oim, PrepaidBagsOrderItem.class ) );  
   ...
  
   return funMap;  
}

private AncillaryOrderItem getAncillaryOrderItemFromObject( Object orderItemObj )  
{  
   var orderItemMap = mapper.convertValue( orderItemObj, LinkedHashMap.class );  
   var currentItemType = ItemTypeEnum.valueOf( orderItemMap.get( "itemType" ) );  
   var fun = orderItemMapper.get( currentItemType );  
   if( fun == null ) throw new BadDataException();  
       
   return fun.apply( orderItemMap ); ;  
}
```

Rather than doing it that way, there's an annotation that exists called `@JsonSubTypes` that will automatically do that mapping during serialization. 

![[Pasted image 20221024161817.png]]

The way to get the `openapi-generator-maven-plugin` to automatically generate those annotations is by providing a discriminator with a list of mappings, and [add the `jersey2` library to the configuration](https://stackoverflow.com/questions/59087711/swagger-generate-subtypes-based-on-enum-value).

``` yaml
discriminator:  
  propertyName: itemType  
  mapping:  
    PREFERRED_SEAT: "#/components/schemas/seatOrderItem"  
    REGULAR_SEAT: "#/components/schemas/seatOrderItem"  
    AUTOASSIGN_SEAT: "#/components/schemas/seatOrderItem"  
    MAIN_CABIN_EXTRA: "#/components/schemas/seatOrderItem"  
    PAID_SEAT: "#/components/schemas/seatOrderItem"  
    TICKET: "#/components/schemas/ticketIssuanceOrderItem"  
    EXCHANGE_TICKET: "#/components/schemas/exchangeTicketOrderItem"  
    REISSUE: "#/components/schemas/ticketReIssuanceOrderItem"  
    REVAL: "#/components/schemas/ticketReIssuanceOrderItem"  
    MILEAGE_REINSTATEMENT_FEE: "#/components/schemas/ancillaryOrderItem"  
    ADHOC_TRIPCREDIT: "#/components/schemas/ancillaryOrderItem"  
    GROUP_DEPOSIT: "#/components/schemas/groupDepositOrderItem"  
    E500: "#/components/schemas/unaffiliatedUpgradeOrderItem"  
    MISC_SVCS_FEE: "#/components/schemas/feeOrderItem"  
    PAYONLY_BOOKED: "#/components/schemas/payOnlyBookedOrderItem"  
    MILEAGE_MULTIPLIER: '#/components/schemas/mileageMultiplierOrderItem'  
    INSTANT_UPSELL: '#/components/schemas/instantUpsellOrderItem'  
    PREPAID_BAGS: '#/components/schemas/prepaidBagsOrderItem'  
    SAME_DAY_FLIGHT_CHANGE: '#/components/schemas/sameDayFlightChangeOrderItem'  
    LOAD_FACTOR_BASED_UPGRADE: '#/components/schemas/loadFactorBasedUpgradeOrderItem'  
    SAME_DAY_STANDBY: '#/components/schemas/standbyOrderItem'  
    ALTERNATE_FLIGHT_STANDBY: '#/components/schemas/standbyOrderItem'  
    PRIORITY_PRODUCT: '#/components/schemas/priorityProductOrderItem'  
    AIRPORT_BAGS_MSR: '#/components/schemas/bagCheckinOrderItem'
```

Once we did that, there was no more need for the functional map, our code was more SOLID, and everything was better.

## Performance Testing

### Tutorials
* Load Generators
* Virtualization (Dev Test, Capacity, Think time)
* [Wiki](https://wiki.aa.com/bin/view/Edge/Rancher/Load%20Testing/)

Tip: Download [Video Speed Controller](https://chrome.google.com/webstore/detail/video-speed-controller/nffaoalbilbmmfgbnbgppjihopabppdk/RK%3D2/RS%3DsdeaBShiWo3AFJdky6bPF3xKL6Y- "https://chrome.google.com/webstore/detail/video-speed-controller/nffaoalbilbmmfgbnbgppjihopabppdk/rk%3d2/rs%3dsdeabshiwo3afjdky6bpf3xkl6y-") extension to **adjust playback speed** of videos.

Load Runner, Performance Center, and Dynatrace:

Visit this [video](https://spteam.aa.com/sites/ITS/eTDS/Edge%20Documents/Videos/Load%20Runner%20VuScript,%20Performance%20Center%20Test,%20Dynatrace%20JVM%20Metrics,%20Buil%20Pipeline%20Java%20Args%20(15-10-2020).mp4 "https://spteam.aa.com/sites/its/etds/edge%20documents/videos/load%20runner%20vuscript,%20performance%20center%20test,%20dynatrace%20jvm%20metrics,%20buil%20pipeline%20java%20args%20(15-10-2020).mp4") for a detailed walk-through of the steps in this Wiki Page.

0:00 - 26:23  -  Load Runner VuScript
26:23 - 36:46  -  Performance Center Test
44:47 - 49:48  -  Dynatrace Navigation
*49:49 - 59:17  -  Dynatrace JVM Metrics
59:18 - 1:10:42  -  Build Pipeline Java Args / Docker Container / Dynatrace JVM Metrics

For Carol: [Gathering Load Testing Results](https://mysite.aa.com/personal/242924_corpaa_aa_com/_layouts/15/onedrive.aspx?originalPath=aHR0cHM6Ly9teXNpdGUuYWEuY29tLzp2Oi9nL3BlcnNvbmFsLzI0MjkyNF9jb3JwYWFfYWFfY29tL0VmdnktSFRpSzB4T29uN3FrbEdBcVNNQk5tRU92Wmo2eXdDN3BwNndFWGZEY0E%5FcnRpbWU9TnN5Z2Q4RnkyRWc&id=%2Fpersonal%2F242924%5Fcorpaa%5Faa%5Fcom%2FDocuments%2FVideos%2FTutorials%2FLoad%20Testing%20Results%20with%20Carol%20%2812%2D01%2D2021%29%2Emp4&parent=%2Fpersonal%2F242924%5Fcorpaa%5Faa%5Fcom%2FDocuments%2FVideos%2FTutorials "https://mysite.aa.com/personal/242924_corpaa_aa_com/_layouts/15/onedrive.aspx?originalpath=ahr0chm6ly9texnpdguuyweuy29tlzp2oi9nl3blcnnvbmfslzi0mjkynf9jb3jwywffywffy29tl0vmdnktsfrpszb4t29un3frbedbcvnnqk5tru92wmo2exddn3bwnndfwgzey0e%5fcnrpbwu9tnn5z2q4rnkyrwc&id=%2fpersonal%2f242924%5fcorpaa%5faa%5fcom%2fdocuments%2fvideos%2ftutorials%2fload%20testing%20results%20with%20carol%20%2812%2d01%2d2021%29%2emp4&parent=%2fpersonal%2f242924%5fcorpaa%5faa%5fcom%2fdocuments%2fvideos%2ftutorials")

Virtualization:
Watch this **[video](https://mysite.aa.com/personal/242924_corpaa_aa_com/_layouts/15/onedrive.aspx?originalPath=aHR0cHM6Ly9teXNpdGUuYWEuY29tLzp2Oi9nL3BlcnNvbmFsLzI0MjkyNF9jb3JwYWFfYWFfY29tL0VmdnktSFRpSzB4T29uN3FrbEdBcVNNQk5tRU92Wmo2eXdDN3BwNndFWGZEY0E%5FcnRpbWU9TnN5Z2Q4RnkyRWc&id=%2Fpersonal%2F242924%5Fcorpaa%5Faa%5Fcom%2FDocuments%2FVideos%2FTutorials%2FVirtualization%2Emp4&parent=%2Fpersonal%2F242924%5Fcorpaa%5Faa%5Fcom%2FDocuments%2FVideos%2FTutorials "https://mysite.aa.com/personal/242924_corpaa_aa_com/_layouts/15/onedrive.aspx?originalpath=ahr0chm6ly9texnpdguuyweuy29tlzp2oi9nl3blcnnvbmfslzi0mjkynf9jb3jwywffywffy29tl0vmdnktsfrpszb4t29un3frbedbcvnnqk5tru92wmo2exddn3bwnndfwgzey0e%5fcnrpbwu9tnn5z2q4rnkyrwc&id=%2fpersonal%2f242924%5fcorpaa%5faa%5fcom%2fdocuments%2fvideos%2ftutorials%2fvirtualization%2emp4&parent=%2fpersonal%2f242924%5fcorpaa%5faa%5fcom%2fdocuments%2fvideos%2ftutorials")**.


Updates since recording:
-   Add Thinktime based on response times observed in logs.
-   If name `matches .*` instead of `contains _`

![](https://eastus1-mediap.svc.ms/transform/thumbnail?provider=spo&inputFormat=png&cs=MWZlYzhlNzgtYmNlNC00YWFmLWFiMWItNTQ1MWNjMzg3MjY0fFNQTw&docid=https%3A%2F%2Fspteam%2Eaa%2Ecom%2F%5Fapi%2Fv2%2E0%2Fdrives%2Fb%21sw%2D5%5FTAK20qY%5FaZ9KrcRwfvH8%5FnWTftLiaGlHe1wx2c9q79BzuqIRrLREqmTudy8%2Fitems%2F01I5WIN4LRUBHIXIKA4VD2UMNTIA6KI3VQ%3Ftempauth%3DeyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0%2EeyJhdWQiOiIwMDAwMDAwMy0wMDAwLTBmZjEtY2UwMC0wMDAwMDAwMDAwMDAvc3B0ZWFtLmFhLmNvbUA0OTc5M2ZhZi1lYjNmLTRkOTktYTBjZi1hZWY3Y2NlNzlkYzEiLCJpc3MiOiIwMDAwMDAwMy0wMDAwLTBmZjEtY2UwMC0wMDAwMDAwMDAwMDAiLCJuYmYiOiIxNjY2NjU2MDAwIiwiZXhwIjoiMTY2NjY3NzYwMCIsImVuZHBvaW50dXJsIjoiUzdMYlBiWFBWbGNTWkZYWGVGQytOaXdsbFREVHhaKzNMR1VtT3hYWG5qZz0iLCJlbmRwb2ludHVybExlbmd0aCI6IjE0NiIsImlzbG9vcGJhY2siOiJUcnVlIiwidmVyIjoiaGFzaGVkcHJvb2Z0b2tlbiIsInNpdGVpZCI6IlptUmlPVEJtWWpNdE1HRXpNQzAwWVdSaUxUazRabVF0WVRZM1pESmhZamN4TVdNeCIsImFwcF9kaXNwbGF5bmFtZSI6Ik1pY3Jvc29mdCBUZWFtcyIsImdpdmVuX25hbWUiOiJFdGhhbiIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiYXBwaWQiOiIxZmVjOGU3OC1iY2U0LTRhYWYtYWIxYi01NDUxY2MzODcyNjQiLCJ0aWQiOiI0OTc5M2ZhZi1lYjNmLTRkOTktYTBjZi1hZWY3Y2NlNzlkYzEiLCJ1cG4iOiIyNDI5MjRAY29ycGFhLmFhLmNvbSIsInB1aWQiOiIxMDAzMjAwMDk0M0VFOUQ2IiwiY2FjaGVrZXkiOiIwaC5mfG1lbWJlcnNoaXB8MTAwMzIwMDA5NDNlZTlkNkBsaXZlLmNvbSIsInNjcCI6Im15ZmlsZXMud3JpdGUgYWxsc2l0ZXMuZnVsbGNvbnRyb2wgYWxsc2l0ZXMubWFuYWdlIGFsbHByb2ZpbGVzLndyaXRlIiwidHQiOiIyIiwidXNlUGVyc2lzdGVudENvb2tpZSI6bnVsbCwiaXBhZGRyIjoiNzYuMjUzLjE2My4yNCJ9%2EMWN6ZFBuWFcyWnJIUmpjZ0tRVWVFK2pJeTNEQ1ozVVU4cUJKY3pjYkpVcz0%26version%3DPublished&width=800&height=800&cb=63745996671)

### Load Runner

5 Steps in updating VuScript:
1.  Update endpoint in `Runtime Settings` > `Additional Attributes`
2.  Update Resource Path in `ETDs_API`
3.  Replace Request Playloads in Extra Files (`F2` to rename)
4.  Update All references to old Payloads ( `Ctrl+H` > `Elevate Scope to Current Script`)
5.  Update transaction IDs in `ETDS API` step to match requests


Testing:
1.  ~~Disconnect from VPN~~
2.  Enable Fiddler Proxy in `vuser_init`
3.  Re-enable Capture Traffic in Fiddler
4.  Increase number of users to 50 in Runtime Settings > Run Logic

Export:
1.  Save Script As
2.  Send Contents to zip

### Performance Center

1.  `Hamburger Menu` > `Testing` > New Folder > MSD > `Upload script` > select Zip file
2.  `New test` > MSD_singleinstance_Test
3.  Select scripts
4.  Edit command line > Add `-cancel_security_mode` > Add to all scripts in test (doesn't matter if only one script)
> 	Error: CCI security error:You are running under secure mode and the function lr_param_sprintf is not allowed in this mode.
5.  `Properties` > Start all Vusers gradually 3 every 2 minutes > Run for 8 hours > Stop 3 every 30 seconds
6.  Select Load Generator > PHX
    -   Custom LG selector: `LG Management` > `Automatch` > `LG1` > Location: `PHX`
7.  Save and Run > 10 hours 30 minutes > 2 hours

Transactions:

-   MatchingTxnIDCountInResponse_1: Number of passed throughput.
-   MatchingTxnIDCountInResponse_0: Number of failed input.

Common Errors:

> Error: CCI security error:You are running under secure mode and the function lr_param_sprintf is not allowed in this mode.

Add `-cancel_security_mode` to script
  
> Continuing after Error -27792: Failed to connect to server “load-airextra-sws-ms.ticketing-non-prod-clus-cf493f47c69a8f9ee98e3c77c3874da6-0000.us-south.containers.appdomain.cloud:80”: [10061] Connection refused

Set Load Generator to select from PHX.

> ETDs_API.c(29): Continuing after Error -26611: HTTP Status-Code=501 (Not Implemented) for "[http://load-airextra-sws-ms.ticketing-non-prod-clus-cf493f47c69a8f9ee98e3c77c3874da6-0000.us-south.containers.appdomain.cloud/api/v1/createAirExtra"](http://load-airextra-sws-ms.ticketing-non-prod-clus-cf493f47c69a8f9ee98e3c77c3874da6-0000.us-south.containers.appdomain.cloud/api/v1/createAirExtra%22 "http://load-airextra-sws-ms.ticketing-non-prod-clus-cf493f47c69a8f9ee98e3c77c3874da6-0000.us-south.containers.appdomain.cloud/api/v1/createairextra%22") [MsgId: MERR-26611] [Time:2021-01-25 09:57:23]

Ensure capacity for Virtualized Services in DevTest is set higher than the load it's receiving
![[5357c81d-70f9-43cd-8caa-be9491068044.png]]

> ETDs_API.c(31): Continuing after Error -26631: HTTP Status-Code=400 (Bad Request) for "http://load-emdmanager-ms.ticketing-non-prod-clus-cf493f47c69a8f9ee98e3c77c3874da6-0000.us-south.containers.appdomain.cloud/api/v1/exchange"   [MsgId: MERR-26631] [Time:2021-01-28 17:35:37]

Replace `¥` characters with `#` in the VU Script payloads.

Other failures:
1.  Ensure Sabre calls are using virtualized end points (may need ClientFactory circumvention in code)
2.  Ensure virtualized services are deployed to [http://169.61.40.114:31507](http://169.61.40.114:31507/ "http://169.61.40.114:31507/") and not [qmoload.cdc.aa.com](http://qmoload.cdc.aa.com/ "http://qmoload.cdc.aa.com/").

### Dynatrace

Navigation:
-   `Transactions and Services` > AOL (select load)
-   Management zone: `TaR_Load`
-   Timeframe Selector: match timestamps of test in performance center (or last 30 minute for real-time)
-   Verify virtualization delays: `Controller` > `View service flow` > `view Pure Paths` > click one
-   JVM metrics: `Controller` > `Services` > `Spring boot application` > `JVM mtrics`
-   2 Services > Application > Apache > SpringBoot

Crucial information:
-   Allocation of memory heaps
-   When the garbage collection activity suspends a process
-   How much memory was utilized during this cycle

#### Service Page

Select `Transactions and Services` and search by service name, then select your `Controller`.

#### Pure Paths

From the Service page, select `View service flow` > `View PurePaths`

#### Process Page

From the Service page select `Apache Tomcat` > `SpringBoot service-name...`

#### Container Page

From the `Process page`, select the linked `Kubernetes cluster` > `View containers` > sort by `CPU` > select your container
  

![](https://eastus1-mediap.svc.ms/transform/thumbnail?provider=spo&inputFormat=png&cs=MWZlYzhlNzgtYmNlNC00YWFmLWFiMWItNTQ1MWNjMzg3MjY0fFNQTw&docid=https%3A%2F%2Fspteam%2Eaa%2Ecom%2F%5Fapi%2Fv2%2E0%2Fdrives%2Fb%21sw%2D5%5FTAK20qY%5FaZ9KrcRwfvH8%5FnWTftLiaGlHe1wx2c9q79BzuqIRrLREqmTudy8%2Fitems%2F01I5WIN4LWG547QALWC5BJCZWK33BJ2QOQ%3Ftempauth%3DeyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0%2EeyJhdWQiOiIwMDAwMDAwMy0wMDAwLTBmZjEtY2UwMC0wMDAwMDAwMDAwMDAvc3B0ZWFtLmFhLmNvbUA0OTc5M2ZhZi1lYjNmLTRkOTktYTBjZi1hZWY3Y2NlNzlkYzEiLCJpc3MiOiIwMDAwMDAwMy0wMDAwLTBmZjEtY2UwMC0wMDAwMDAwMDAwMDAiLCJuYmYiOiIxNjY2NjU2MDAwIiwiZXhwIjoiMTY2NjY3NzYwMCIsImVuZHBvaW50dXJsIjoieUJ1cFBNNGpDRDBPNGtxWnJtNWZualpVWHJ4RjA4Y1U4ZXdEZ0FNbmt5RT0iLCJlbmRwb2ludHVybExlbmd0aCI6IjE0NiIsImlzbG9vcGJhY2siOiJUcnVlIiwidmVyIjoiaGFzaGVkcHJvb2Z0b2tlbiIsInNpdGVpZCI6IlptUmlPVEJtWWpNdE1HRXpNQzAwWVdSaUxUazRabVF0WVRZM1pESmhZamN4TVdNeCIsImFwcF9kaXNwbGF5bmFtZSI6Ik1pY3Jvc29mdCBUZWFtcyIsImdpdmVuX25hbWUiOiJFdGhhbiIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiYXBwaWQiOiIxZmVjOGU3OC1iY2U0LTRhYWYtYWIxYi01NDUxY2MzODcyNjQiLCJ0aWQiOiI0OTc5M2ZhZi1lYjNmLTRkOTktYTBjZi1hZWY3Y2NlNzlkYzEiLCJ1cG4iOiIyNDI5MjRAY29ycGFhLmFhLmNvbSIsInB1aWQiOiIxMDAzMjAwMDk0M0VFOUQ2IiwiY2FjaGVrZXkiOiIwaC5mfG1lbWJlcnNoaXB8MTAwMzIwMDA5NDNlZTlkNkBsaXZlLmNvbSIsInNjcCI6Im15ZmlsZXMud3JpdGUgYWxsc2l0ZXMuZnVsbGNvbnRyb2wgYWxsc2l0ZXMubWFuYWdlIGFsbHByb2ZpbGVzLndyaXRlIiwidHQiOiIyIiwidXNlUGVyc2lzdGVudENvb2tpZSI6bnVsbCwiaXBhZGRyIjoiNzYuMjUzLjE2My4yNCJ9%2EKzZjc3h6ZEJjU0R2a0p3UTJvUlIvMlJTRVZ5dGJwZDVNZXd2cEJ6cU5QQT0%26version%3DPublished&width=800&height=800&cb=63738990466)

### JVM Metrics / Java Arguments

_Eden Space_ (also known as Young Generation):
-   The space allocated for all the temporary (newly created) objects (method variables, etc)  

_G1 Old Gen_:
-   The space allocated for long-living objects (singletons, global constants, etc.)  

_Suspension_:
-   A measure of the unresponsiveness of the application during garbage collections

_Garbage collection count_:
-   Number
-   When memory usage reaches max heap memory (-Xmx768m), garbage collection suspends application threads to clear unneeded memory for a short time.

_Thread_:
-   The path followed when executing the program.

_Threads_:
-   The number of tasks being handled in parallel.

Example of [Full Garbage Collection (FGC) cycles](https://apm.aa.com/e/cff28573-f815-4847-935f-ba99fd7e336f/#processdetails;id=PROCESS_GROUP_INSTANCE-E8B7F01BA802302E;gtf=2020-10-13T09:14:00-05:00%20to%202020-10-13T17:30:00-05:00;gf=-8112610223964939191 "https://apm.aa.com/e/cff28573-f815-4847-935f-ba99fd7e336f/#processdetails;id=process_group_instance-e8b7f01ba802302e;gtf=2020-10-13t09:14:00-05:00%20to%202020-10-13t17:30:00-05:00;gf=-8112610223964939191").
![[337cde8e-af73-43f6-b05e-6a0ccafb22c9.png]]
```
A Full GC will be triggered whenever the heap fills up. In such a case the
young generation is collected first followed by the old generation. If the
old generation is too full to accept the content of the young generation,
the young generation GC is omitted and the old generation GC is used to
collect the full heap, either in parallel or serial. Either way the whole
heap is collected with a stop-the-world event.
```

Stack and Heap: Stack is Last In, First Out (Like plates) for method calls.

Memory Kills:
Max Container Memory should be higher than Max JMV Heap Memory. If JVM uses 1GB memory when max container allocation is 1GB, then no memory is left for CPU operations, which will result in memory kills.

> JAVA_OPTIONS: -XX:+UseG1GC -XX:+UseStringDeduplication -XX:MaxGCPauseMillis=200 -XX:MaxMetaspaceSize=128M -Xms256m -Xmx768m

References:
-   [https://answers.dynatrace.com/questions/176792/jvm-heap-metrics-on-ruxit.html](https://answers.dynatrace.com/questions/176792/jvm-heap-metrics-on-ruxit.html "https://answers.dynatrace.com/questions/176792/jvm-heap-metrics-on-ruxit.html")
-   [https://www.eginnovations.com/blog/java-threads/](https://www.eginnovations.com/blog/java-threads/ "https://www.eginnovations.com/blog/java-threads/")
-   [https://www.baeldung.com/jvm-parameters](https://www.baeldung.com/jvm-parameters "https://www.baeldung.com/jvm-parameters")

### Breakpoints

Breakpoints:
-   If response time is too slow (due to high suspension ~5%).
-   If memory kill causes an application restart
-   If any problems

#### Example of [Memory Kill](https://apm.aa.com/e/cff28573-f815-4847-935f-ba99fd7e336f/#customdevicegroupdetails/entity;id=CONTAINER_GROUP_INSTANCE-1F818407470F2B00;gtf=2020-10-22T00:00:00-05:00%20to%202020-10-22T01:00:00-05:00;gf=-8112610223964939191 "https://apm.aa.com/e/cff28573-f815-4847-935f-ba99fd7e336f/#customdevicegroupdetails/entity;id=container_group_instance-1f818407470f2b00;gtf=2020-10-22t00:00:00-05:00%20to%202020-10-22t01:00:00-05:00;gf=-8112610223964939191").
![[5dcb62e2-d805-4330-b280-92331f41ae98.png]]
Solution: decrease max memory heap in Java args (-Xmx).

#### Example of [High Suspension](https://apm.aa.com/e/cff28573-f815-4847-935f-ba99fd7e336f/#smgd;sci=SERVICE-2613010FFF24D00C;tab=RT;servicefilter=0%1E10%11SERVICE_METHOD_GROUP-6170ED52864DF885;gtf=2020-10-22T10:00:00-05:00%20to%202020-10-22T11:28:00-05:00;gf=-8112610223964939191;timeframe=custom1603383780000to1603384080000 "https://apm.aa.com/e/cff28573-f815-4847-935f-ba99fd7e336f/#smgd;sci=service-2613010fff24d00c;tab=rt;servicefilter=0%1e10%11service_method_group-6170ed52864df885;gtf=2020-10-22t10:00:00-05:00%20to%202020-10-22t11:28:00-05:00;gf=-8112610223964939191;timeframe=custom1603383780000to1603384080000").
![[ab2dc409-b692-46af-a1d8-dc788c67e221.png]]
![[fa62db7a-ebd6-4932-9f2b-11347fcbbec7.png]]

Solution: increase max memory heap in Java args (-Xmx) or simply reduce load.

#### Example of [High Garbage Collection time per thread](https://apm.aa.com/e/cff28573-f815-4847-935f-ba99fd7e336f/#processdetails;id=PROCESS_GROUP_INSTANCE-36FE36710A7D4AF6;gtf=2020-10-22T10:00:00-05:00%20to%202020-10-22T11:28:00-05:00;gf=-8112610223964939191 "https://apm.aa.com/e/cff28573-f815-4847-935f-ba99fd7e336f/#processdetails;id=process_group_instance-36fe36710a7d4af6;gtf=2020-10-22t10:00:00-05:00%20to%202020-10-22t11:28:00-05:00;gf=-8112610223964939191").

![[5a4d9b5f-3b0b-4801-bb3b-fc8a9aba1e9d.png]]

Solution: reduce number of objects being created per thread in your code. 

#### Example of Running Vusers during Failures.

![[353a747d-1fa0-4438-8916-52ff26468344.png]]
From Performance Center Results:
1.  Click `+ Add` by `Merged Graphs`.
2.  Add `Running Vusers`
3.  Add `HTTP Responses per Second`
    1.  Or add `Trans/Sec (Passed)`
    2.  And `Trans/Sec (Failed)`
4.  Select `Show only me`, `Show`, and `Show` respectively.

![[52001e00-1dfe-497c-b888-67ea10da29c7.png]]


### Java Profiling

Installation:
1.  Download [YourKit](https://www.yourkit.com/java/profiler/download/ "https://www.yourkit.com/java/profiler/download/") profiler
2.  Use the license key **YLEGZC5JKBJXLHAJ9D0R0** to activate.  

Use:
1.  Run local Tomcat.
2.  Select your service in YourKit.
3.  Navigate to `CPU` > `Call tree`

![](https://eastus1-mediap.svc.ms/transform/thumbnail?provider=spo&inputFormat=png&cs=MWZlYzhlNzgtYmNlNC00YWFmLWFiMWItNTQ1MWNjMzg3MjY0fFNQTw&docid=https%3A%2F%2Fspteam%2Eaa%2Ecom%2F%5Fapi%2Fv2%2E0%2Fdrives%2Fb%21sw%2D5%5FTAK20qY%5FaZ9KrcRwfvH8%5FnWTftLiaGlHe1wx2c9q79BzuqIRrLREqmTudy8%2Fitems%2F01I5WIN4IF4V2S7VJJ7RFJAHLZZE5RW32K%3Ftempauth%3DeyJ0eXAiOiJKV1QiLCJhbGciOiJub25lIn0%2EeyJhdWQiOiIwMDAwMDAwMy0wMDAwLTBmZjEtY2UwMC0wMDAwMDAwMDAwMDAvc3B0ZWFtLmFhLmNvbUA0OTc5M2ZhZi1lYjNmLTRkOTktYTBjZi1hZWY3Y2NlNzlkYzEiLCJpc3MiOiIwMDAwMDAwMy0wMDAwLTBmZjEtY2UwMC0wMDAwMDAwMDAwMDAiLCJuYmYiOiIxNjY2NjU2MDAwIiwiZXhwIjoiMTY2NjY3NzYwMCIsImVuZHBvaW50dXJsIjoiajNHT3pHMnIrZjdpckp2Z3A2ZnVyb1ZGOUdpL3p0SURaL2txSnNTbzk4Zz0iLCJlbmRwb2ludHVybExlbmd0aCI6IjE0NiIsImlzbG9vcGJhY2siOiJUcnVlIiwidmVyIjoiaGFzaGVkcHJvb2Z0b2tlbiIsInNpdGVpZCI6IlptUmlPVEJtWWpNdE1HRXpNQzAwWVdSaUxUazRabVF0WVRZM1pESmhZamN4TVdNeCIsImFwcF9kaXNwbGF5bmFtZSI6Ik1pY3Jvc29mdCBUZWFtcyIsImdpdmVuX25hbWUiOiJFdGhhbiIsImZhbWlseV9uYW1lIjoiTWlsbGVyIiwiYXBwaWQiOiIxZmVjOGU3OC1iY2U0LTRhYWYtYWIxYi01NDUxY2MzODcyNjQiLCJ0aWQiOiI0OTc5M2ZhZi1lYjNmLTRkOTktYTBjZi1hZWY3Y2NlNzlkYzEiLCJ1cG4iOiIyNDI5MjRAY29ycGFhLmFhLmNvbSIsInB1aWQiOiIxMDAzMjAwMDk0M0VFOUQ2IiwiY2FjaGVrZXkiOiIwaC5mfG1lbWJlcnNoaXB8MTAwMzIwMDA5NDNlZTlkNkBsaXZlLmNvbSIsInNjcCI6Im15ZmlsZXMud3JpdGUgYWxsc2l0ZXMuZnVsbGNvbnRyb2wgYWxsc2l0ZXMubWFuYWdlIGFsbHByb2ZpbGVzLndyaXRlIiwidHQiOiIyIiwidXNlUGVyc2lzdGVudENvb2tpZSI6bnVsbCwiaXBhZGRyIjoiNzYuMjUzLjE2My4yNCJ9%2ELzNrN3lMcEMzQnhHbURvaEd3MXZNNWUwQnlqWHppQVFpRnJFL3ZweU1PVT0%26version%3DPublished&width=800&height=800&cb=63738911607) 

Information on getting started and how use the profiler for local and remote profiling: [YourKit Java Profiler Help](https://www.yourkit.com/docs/java/help/ "https://www.yourkit.com/docs/java/help/")
Additional Demos and knowledge base: [Docs Demos Knowledge Base](https://www.yourkit.com/docs/ "https://www.yourkit.com/docs/")
  
Please feel free to reach out to us via Cherwell ticket for any licensing and installation issues, or if you need help with the profiler or troubleshooting issues.

#### **Java Profiling with VisualVM**
Download [VisualVM Launcher](https://plugins.jetbrains.com/plugin/7115-visualvm-launcher "https://plugins.jetbrains.com/plugin/7115-visualvm-launcher") for IntelliJ, the add the path to `%JAVA_HOME%/bin` and press `j` to find `jvisualvm.exe`.

### Common Errors
High resource usage with no performance when autoscaling new pods
  

  

Fiddler:

Watch this **[video](https://mysite.aa.com/personal/242924_corpaa_aa_com/_layouts/15/onedrive.aspx?originalPath=aHR0cHM6Ly9teXNpdGUuYWEuY29tLzp2Oi9nL3BlcnNvbmFsLzI0MjkyNF9jb3JwYWFfYWFfY29tL0VmdnktSFRpSzB4T29uN3FrbEdBcVNNQk5tRU92Wmo2eXdDN3BwNndFWGZEY0E%5FcnRpbWU9TnN5Z2Q4RnkyRWc&id=%2Fpersonal%2F242924%5Fcorpaa%5Faa%5Fcom%2FDocuments%2FVideos%2FTutorials%2FLoad%20Testing%2Emp4&parent=%2Fpersonal%2F242924%5Fcorpaa%5Faa%5Fcom%2FDocuments%2FVideos%2FTutorials "https://mysite.aa.com/personal/242924_corpaa_aa_com/_layouts/15/onedrive.aspx?originalpath=ahr0chm6ly9texnpdguuyweuy29tlzp2oi9nl3blcnnvbmfslzi0mjkynf9jb3jwywffywffy29tl0vmdnktsfrpszb4t29un3frbedbcvnnqk5tru92wmo2exddn3bwnndfwgzey0e%5fcnrpbwu9tnn5z2q4rnkyrwc&id=%2fpersonal%2f242924%5fcorpaa%5faa%5fcom%2fdocuments%2fvideos%2ftutorials%2fload%20testing%2emp4&parent=%2fpersonal%2f242924%5fcorpaa%5faa%5fcom%2fdocuments%2fvideos%2ftutorials")**.


## Cucumber

Cucumber is a testing Framework written in Java that allows you to create English sentences that act as the **business requirements** for a feature and double as test **steps for automation**.

* Reflect
* Template Injection
![[Pasted image 20221024212012.png]]
Whenever we are working on a User Story, we’re writing code that’s delivering some business feature, and so we want to record all of those business requirements somewhere so that after we’re finished, we have a giant document that outlines all of the expected behavior of AOL. Cucumber allows us to write out all those feature requirements with the business, and then those business sentences double at automated tests.

- Gherkin is the language that cucumber understands, it is a business Readable, domain specific language that lets you describe software behavior without detailing its implementation.
	
- Gherkin serves two purposes- Documentation and automated tests.
	
- Glue code works as a bridge between spoken language into your application that you want to develop. In our case, it translates English into Java.
    
- The feature is made up with scenarios and steps, and each step essentially maps to the step definition.
	
* It matches the Gherkin sentence with the string inside of this @When annotation and executes the Java code (Glue code) inside if the matching method.
	
* Scenario Outlines: If you don’t want to copy and paste an entire scenario that has only a small difference from an existing scenario, you can use Scenario Outlines.
	
* Output Parameters: The point here is that we want to make our step definitions as reusable as possible, so using parameters, if I want a PNR with 1P2S, or 2P1S, I can use the same test step for any PNR scenario I want. Ticketed, non-ticketed, etc. And that way we don’t have to write a brand-new step definition for every caveat and change. Cucumber can not only match a direct string, but a pattern using Regular Expressions or [Cucumber Expressions](https://github.com/cucumber/cucumber-expressions#readme). If I want to pass data into the step definition, I can do that using output parameters.
	
* Data Tables: Data Tables are handy for passing a list of values to a step definition. For cases when you want to pass more data to a step than fits on a single line.
	

Benefits of Cucumber:
1. Merging branches together will be easier. In ReadyAPI you can overwrite, but not merge.
2. Reusing code and organize everything well.
3. Eliminating small windows in ReadyAPI GUI.
4. It's Java based (less learning curve for newcomers and more versatile).
5. It's open source (no licensing fee).
6. Less repeated information (readyAPI forces you to duplicate the same boilerplate requests over each test case).
7. ReadyAPI’s Dark Mode needs some work.
8. Seamless Git integration. Stop sending documents around and worrying if everyone has the latest version.

[T&R Bootcamp - Cucumber](https://spteam.aa.com/sites/Ticketing_Devs/Shared%20Documents/Forms/AllItems.aspx?id=%2Fsites%2FTicketing%5FDevs%2FShared%20Documents%2Fbootcamp&viewid=df54c38c%2D6768%2D4020%2Da610%2D99bddf554683)

We created a POC a evaluate the pros and cons of moving from ReadyAPI to this Java Framework, and eventually the whole team transitioned.