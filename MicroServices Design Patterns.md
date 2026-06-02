====================================================================================
Microsevices design patterns
https://www.edureka.co/blog/microservices-design-patterns#APIGateway
https://javarevisited.blogspot.com/2021/09/microservices-design-patterns-principles.html#axzz7yNamUPOr
MicroServices DesignPatterns 
	1. Aggregator (if you need info from 2 or 3 services we go with this) DRY Principal
	2. API Gateway (Entry point to clients request and using load balancer call will be diverted to respective microservices)
	3. CHAIN OF RESPONSIBILITY ( produces a single ooutput which is combination of multiple microservices)(draw back, shouldn't use because when wehave 20 servies then response deplay we see)
	4. asynchronus messaging (Here it is oppsite above, here all the services communicate with each other but not serially so we can go with this)
	5. DATABASE = Database Per service or shared database.
	6. Event Sourcing = Creates events regarding the changes in application state
	7. Branch	=	Simultaneulsy process the request and response on multiple microservices
	8. CQRS: Command query responsibility segregator = Command will be in one microserves(create , update , datelet ) and query realted will be in another MS (select *..)
	9. Circuit Breaker	= Inorder to send customixed message when any of the MS is down this will be used(this can be achieved by hystirx)
	10. Decomposition	= Breaking up monolithic to microservices.
	
12 factors of microservies: Codebase (version control), 
		dependencies (version number should be dynamic but not hardcoded),  
		Config (dev, stage), 
		Backing Services(Data base consuming)
		Build, Release and Run
		Port Binding ( 8080)
		Concurrency
		Disposability - Even if any error comes in applciaion , it should fail-safe and also it should fast startup .
		Development/Production Parity ( All tech, emrmoligies should be same in non prod and prod)
		Logs
		Admin Processes
====================================================================================
Spring MVC:

1)Spring Framework + XML configurations + External server.

	Spring 4.x
	2)JSP or HTML file,--> web.xml -->dispatcher servlet.xml -->controller layer -->service layer -->Data Access Object layer-->data base.

	Spring 5.x
	3)JSP or HTML file,--> appconfig.java -->controller layer -->service layer -->Data Access Object layer-->data base.

	Above both are web applciations, we cannot create a jar file to run this application, compulsory we need external server to run this. We will run the project 
	through the WAR file.

	Dynamic web project or maven project
Spring Boot:
1)Spring Framework - XML configurations + internal server.

	JSP or HTML -->controller layer -->service layer -->Data Access Object layer-->data base.
	Internally we have 3 servers tomcat/jetty/under tier.
	
	Advantage:
	It is a standalone applciations.
	It also supports microservices and cloud.
	It supports third party libraries also (Distributed Cache and etc..)
	Internal server.
	
	How many way we can create the object:
		1) Ecllipse maven 
		2) Spring Intializer (start.spring.io)
		3) STS
	If you want to run through command prompt:
		java -jar name.jar
=================================================================================================================================================================
Types Of Autowiring

			public class Car{	
			public Wheel wheel; 

			public Car(Wheel w1)
			{
			this.wheel = w1;
			}
			}
			public class  Wheel { private String name }

			default
			
			byName

			<bean name ="car", class ="Car"  autowire="byName"/>
			<bean name="Wheel" class="Wheel">
			<property name="name", value="Wheel1"/>

			byType

			<bean name ="car", class ="Car"  autowire="byType"/>
			<bean name="alloyWheel" class="Wheel">
			<property name="name", value="Wheel1"/>

			Constructor

			<bean name ="car", class ="Car"  autowire="constructor"/>
			<bean name="alloyWheel" class="Wheel" autowire="byName>
			<property name="name", value="Wheel1"/>
=====================================================================================================================================================
Type of Dependency Injections:
https://www.javacodegeeks.com/2019/02/field-setter-constructor-injection.html
https://www.amitph.com/spring-setter-injection-example/
	Constuctor Based
	Setter Based injection
	Field injection
	
	Setter Based:
	Difference betweeen Constructor and Setter autowired innections is		
	@Autowired(required="false") //this will not throw any exception if Dog bean is not there in bean. (But when we hit api, it will fail)
	public void setAnimal(@Qualifier("dog") Animal animal)
	this.animal = animal;
	
	Constructor based:
	@Autowired(required="false") //this will throw exception if Dog bean is not there in bean while starting the application
	public Animal(@Qualifier("dog") Animal animal)
	this.animal = animal;
=================================================================================================================================================================
Spring scopes
https://www.amitph.com/spring-bean-scopes-guide/#Spring_Bean_Scopes

			Singleton (default)
				only one object will  be created.
				@Scope(value = ConfigurableBeanFactory.SCOPE_SINGLETON)
				@Component
				public class DogsDao {}
			Prototype	
				Everytimne new  object will  be created.
				@Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
				@Component
				public class DogsDao {}
			Request
				1)When evera request comes, the only this bean will be created.
				2)But by defauly, one bean has to createa at the start of the apllication inorder to injec the dependecy in the dependent class	
					So , we use this proxy, container will create a dummy bean adn when ever a reqest comes then it will replace with that one.
				@Component
				@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
				public class DogDao {
				
Session   		Bean will be alive until the session is expired
Application		Bean will be alive until the applcaition is destoyed
=================================================================================================================================================================
=================================================================================================================================================================

Microservices Architecture
1)service discovery eureka client
1)service eureka server

OrderService  
		@EnableEurekaClient Annotation
		Spring-Cloud-Eureka-Discovery-Client,Spring-Cloud-Dependencies (registery)
		Spring-Cloud-Started-Cofig , cloud.config.url of springcofigserver (Iorder to get propeties from git repo it talks to another microserice)

PaymentService  
		@EnableEurekaClient Annotation
		Spring-Cloud-Eureka-Discovery-Client,Spring-Cloud-Dependencies (registery)
		Spring-Cloud-Started-Cofig , cloud.config.url of springcofigserver (Iorder to get propeties from git repo it talks to another microserice)
				
ServiceRegistery 
		@EnableEurekaServer   It is client	, register with eureka server will be false.
		spring-cloud-eureka-server. this is the dependency
		
Spring Cloud GateWay
		@EnableEurekaClient Annotation
		@EnableHystrix	(Circuit breaker.)(Gateway dependency and zull dependency (not using now)) and @HystrixCommand (method annotation)
		1) 	Here we will add routing in app.properites inorder to hit partiuclar service.
		2)	Hystricx Dashboard --> This for fall bac tollerance. when ever aany service is down, this histrix will send an customixed messsage to taht service.
			Here it is slef we will add the sprin-cloud -starter-netflix histrix dependency. FallBack controllers for order service and payment service.
			SO we will add this all stuff in the same above applciation poperties. filters.name=CircuitBreaker and enable the histrix in app,pro include=true.
			
HistrixDashBoard application.
		@EnableHystrixDashboard	which shows numbers of faiures in the aplciations client call. 

SpringConfigServer
		This will talk to git repo and get the common propertis and share to multiple services.
		@EnableEurekaClient  and @EnableConfigServer 
		Spring-Cloud-Eureka-Discovery-Client,Spring-Cloud-Dependencies , spring-cloud-config-server
		All other servce who need common propertis should add this dependecu

ELK = ElasticSearch (no sql data base, log stash(take the logs and exports to kibana process the data, Kibana.
		logstash --> ElasticSearch storage ->kibana
		
Spring Cloud Slueth and zipkins to perform dstributed log tracing.
		spring-cloud-starter-sleuth spring-cloud-starter-zipkin
		zipkin.base-url=localhost=9091
		
Spring Cloud zuul server - Which acts as apigateway and routing (prefilters, postfilters, router filter, error filter) exntends ZuulFilter
			DoctorSercvice, DiagnoiseService, HospitalGateway

SpringCloudFiegnClient and SpringCloud RibbonClinet
		Feign client by default supports client side load balancing by using Ribbon and fault tolerance by using Hystrix.
		In fiegn , we will just create a n interface, implimentation is default.
		@EnableFeignClients, @FeignClient(url="https://jsonplaceholder.typicode.com",name="USER-CLIENT")
		
		@RibbonClient(name = "chatbook", configuration = RibbonConfiguration.class)
		@LoadBalanced 
		public RestTemplate template() {return new RestTemplate();}
		Ribbon API can determine the server's availability through the constant pinging of servers at regular intervals and has a capability of skipping the servers which are not live.
		Amazon Server, AmazonShopping, PaymentService

Spring Pivotal Cloud Factory:
		Here, insteadof eureka server  as a service registry, we will use pivotal cloud factory as a one service registry
		https://www.youtube.com/watch?v=jM2HwFbVtIg
		https://www.youtube.com/watch?v=HMBupaa1P1A
		IAAS - Infrasturcre as a service	Ex: AWS (applitn, data, runtime, middleware)
		PASS	-	Platform as a service.	Ex: Pivotal Cloud, Google Cloud(appltn, data)
		SAAS	-	Software as a service	Ex:	Google Docs
===================================================================================
n+1 hibernate problem spring-hibernate-query-utils this is jar for hibernate to fix n+1 problems

class Department
		@ManyToOne(mappedBy="deparment" , cascade=CascadeType.PERSIST)  (Means many departmens can use same employee
		private List<Employees> 
class Employee
		@OneToMany	(Mean Same employee can be used in many deparments
		@JoinColumn(name=department_id)
		private Department department

//List<Department> departments = session.createQuery("From Department", Department.class).getResultList();
List<Department> departments = session.createQuery("From Department d JOIN fetch d.employees", Department.class).getResultList();


When ever we save the parent then childrean also save by using that cascade persist.
persist.save(deperaent1)

or
Customer   
@OneToMany(targetEntity=Product.class, cascade=CascadeType.All)
@JoinColumn(name="cp_fk", referencedColumn="id ")
List<Product>

Products  
@ManyToOne
=================================================================================================================================================================
Actuator EndPoints
Mappings =Al controllers apis
info   => project details
health ==> h2 data base is up or not , internet coects is up ornot
beans  ==>how many beans we hace
env	=>	
configproperties
Metrics
Loggers
scheduledtasks
===============================================================================================================================================================
Microservices principle ( independent service, scalability,  Decentralization, 
Resilent Service(one service is dowen but all others will be up), Load balancing
						availability, ci-cd devops,
===================================================================================				
Microservices Tranaction mamngement can be used by Distributed Transactions


https://medium.com/swlh/handling-transactions-in-the-microservice-world-c77b275813e0#:~:text=How%20it%20works%3A,transaction%20coordinator%20to%20all%20microservices.

The transaction is now across multiple databases via multiple systems, it is now considered a distributed transaction
	issue: But in distributed transacions, we will loose the ACID nature

Ecommerce Orchestar,
Order MicroService, Inventory MicroService

Solutions by:
Two-Phase Commit:
		TransactionCoordinator  will first begin a global transaction with all the context information.
		OrderMicroservice		prepare command to create an order
		InventoryMicroservice	to reserve the items.
		
		the services are OK to perform the change, they lock down the objects from further changes and notify the TransactionCoordinator.
		
		Thean i will say commt the order , then both will commit and notify the transaction cordinator, then allother obects are unlocked.
		
		BUt here it is synchornous

Eventual Consistency and Compensation / SAGA -> This is asyncronous
		https://www.youtube.com/watch?v=WnZ7IcaN_JA
		Orchestrator Service -Each service publishes an event whenever it updates its data. Other service subscribe to events. When an event is received, a service updates its data.
		OR 
		Chroeography
		UBSER EATS = OrderService, PaymentService, RestaurentService, DeliveryService
=============================================
Map = Data Transformation -> stream.of({'a', 'b'})-> converting lower to upper is called Data Transformation
   - One to One Mapping.
Flatmap = Data Transformation + flattering( Convertring stream of streams into single stream) 
 { {a, b},{c,d},{e,f}}  -> {a,b,c,d,e,f}
  - One to Many Mapping.
  
  
 
Sprint Reactive..
https://www.youtube.com/watch?v=bXcFCgQsvAE&list=PLVz2XdJiJQxyB4Sy29sAnU3Eqz0pvGCkD&index=12
Main advantage of reactive programing is asynchrnous and unblocking flow. 
Publisher, Subscriber, Subscription, Request, OnNexxt(), OnComplete(), OnError()
	1.Subscriber subscribe to the publisher by calling the subscribe method.
	2.Publisher then send the subscription() event to the publisher that it is subscribed.
	3.Request n event is invoked from subscriber to the publisher.
	4.Based on number of request, those many onNext() will be invoked by publisher to produce the results.
	5.If all request are passed then oncomplete() method will be invoked or onError() will be called by Publisher.

==========================================

What optimistic locking & pessimistic locking
Spring-ioc-container, Types of dependency injections, Types of autowiring

Spring boot auto-configuration
Spring boot bean scopes
Spring boot batch
Spring boot swagger- @EnableSwagger2 Swagger is widely used for visualizing APIs, and with Swagger UI it provides online sandbox for frontend developers
Spring boot JPA/HIBERNATE (n+1 related prblems)
Spring boot desgin patterns
Spring boot trans manag (concrncy) monilithic and Distributed trans SAGA MS architechture (Event based (choreography) and command based (orchestration))
			https://www.javainuse.com/spring/boot-transaction-propagation
			
Spring boot security - JWT , OAUTH2
Spring boot accutators

Spring cloud 	pivotal foundry	
				https://www.youtube.com/watch?v=jM2HwFbVtIg
				https://www.youtube.com/watch?v=HMBupaa1P1A
Spring cloud 	zuul server.(api gatewasy) routing 
				https://www.youtube.com/watch?v=6aG0xFpeNFw
				https://www.youtube.com/watch?v=cRhoODRCZAo
				API Gateway with MS https://www.youtube.com/watch?v=bRBgVMngHcQ
Spring cloud 	config
Spring cloud 	eureka (netflix server) (service registry) 
				discovery (service registry)
				https://www.youtube.com/watch?v=irBEdp7XlSQ
Pivotal cloud 	Foundry	= Circuit breaker -  @EnableCircuitBreaker
				hystirx
Spring cloud 	ribbon	(Load balancer (distributing the request)) 
				https://www.youtube.com/watch?v=ueyVjOnDHYQ 
				https://www.youtube.com/watch?v=_e97OLyEEiA
				https://www.baeldung.com/spring-cloud-rest-client-with-netflix-ribbon
				
				Feign client (auto load balancer, mostly we will use it in MS) spring-cloud-starter-openfeign 	
				https://www.youtube.com/watch?v=_MMf2SvNqxo
Spring cloud 	slueth
				zipkins
Spring cloud 	pipelines

MicroServices DesignPatterns
Microservices principles

Spring AOP = Aspect Oriented Language
			
Sprint Reactive Programming
Spring Batch  = When we transfor huge number of data form one source to another source, then this batch processing will be used for quick completion.
				
				https://youtu.be/hr2XTbKSdAQ?list=PLVz2XdJiJQxyC2LMLgDjFGJBX9TJAM4A2
=================
Java

kafka
ExceptionHandling
Multithreaading
Java8 features
Java design patterns
SOLID Principal			
		https://www.baeldung.com/solid-principles
		https://www.youtube.com/watch?v=BM_lSZPMClo

12 factors of SpringBootMicroservices	
		https://www.baeldung.com/spring-boot-12-factor
		https://12factor.net/
Clean code principles 	
		https://www.baeldung.com/java-clean-code
Collections
Abstration,Association,Aggregation,Composition

SQL VS NoSQL DataBases:
		https://www.youtube.com/watch?v=hainZwgvGYY&list=PLVz2XdJiJQxwS8FyWnWyKyfILxHPLsiro&index=2

Java 8 = CompletableFuture interface https://www.youtube.com/watch?v=GJ5Tx43q6KM
BDD 	= https://www.youtube.com/watch?v=8AEQt4CSX5U&t=32s
TDD		= https://www.youtube.com/watch?v=UzRa5cLma0g&t=701s
================================================================================
Spring Boot

@SpringBootApplication = @Configuration @EnableAutoConfiguration(It will enable based oncndtion) @ComponentScan-scan of pur packages to register in bean IOC 

@Conditional or @Configuration (It creates a bean object in IOC Container)
@Controller (with prototypename) or @Service @ @Component("laptop") @Repository
@Primary(class level anotation)

@Autowired	(DependencyInjection)
@Quliaifer("Cat") or @Qualifier("laptop") (We wlill use inthe case of ambiguuty)
@Resource(name="${beanName}")( Ambiguioty + DI)
@Value("${listOfValues.value.fusion.api}")
ConfigurableApplicationCOntext cnotezrt
context.getBean(lien.class)
@Transactional (method level or class level)
@EnableSpringTransactional


Sprint Annoataions:
https://www.youtube.com/watch?v=htyq-mER0AE
===================================================================================
Spring Boot:
Excetion : RestControllerAdvice https://www.youtube.com/watch?v=gPnd-hzM_6A&list=RDCMUCORuRdpN2QTCKnsuEaeK-kQ&index=29
Spring Boot Logging with log4-spring.xml and Splunk.
https://www.youtube.com/watch?v=VO20SgiTTOU&list=PLVz2XdJiJQxwLTK4h387zBtxyKwMkhTXU
===================================================================================
Sprin Auto Configration
@ComponentScan @Configuration @EnableAutoConiguration	
===================================================================================
Sprint AOP = @PointCut and @Advice,, @jointpoint  methods( @AroundAdvice) , @Aspect, 
	Aspect oriented language means, we have to segerate the business logic and our custom logic seperately i.e logs
		    @Pointcut("execution(* Operation.*(..))") 
			@AfterThrowing(pointcut = "execution(* Operation.*(..))",  throwing= "error")  
==========================================================================================================================
Important
<parent>
org.springframework.boot
spring-boot-starter-parent
2.4.5
</parent>

So when we use above parent in pom.xml, then all the dependcies version will use above delcared version. and also parent tag provides configure some default
like java version, webversion and some etcc.

Now what we don't want to specify the parent in web.xml, then we have to come with below approach called spring dependency managment,

<dependencyManagement>
	<dependencies>
	<dependency>	
		org.springframework.boot
		spring-boot-dependencies
		2.4.5
	</dependency>
	</dependencies>
</dependencyManagement>
so adding above tag will act as a parent thing, so if you wanted to use any other sprint dependcies, then version will inherit from above dependency management

==============================================
What is inversion of control:  Refer below for every interview.It is very much important
https://www.educative.io/answers/what-is-inversion-of-control 