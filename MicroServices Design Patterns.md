==========================================================================================================================================================
Microsevices design patterns
https://www.edureka.co/blog/microservices-design-patterns#APIGateway
https://javarevisited.blogspot.com/2021/09/microservices-design-patterns-principles.html#axzz7yNamUPOr
MicroServices DesignPatterns 
	Aggregator (if you need info from 2 or 3 services we go with this) DRY Principal
	API Gateway (Entry point to clients request and using load balancer call will be diverted to respective microservices)
	CHAIN OF RESPONSIBILITY ( produces a single ooutput which is combination of multiple microservices)(draw back, shouldn't use because when wehave 20 servies then response deplay we see)
	asynchronus messaging (Here it is oppsite above, here all the services communicate with each other but not serially so we can go with this)
	DATABASE = Database Per service or shared database.
	Event Sourcing = Creates events regarding the changes in application state
	Branch	=	Simultaneulsy process the request and response on multiple microservices
	CQRS: Command query responsibility segregator = Command will be in one microserves(create , update , datelet ) and query realted will be in another MS (select *..)
	Circuit Breaker	= Inorder to send customixed message when any of the MS is down this will be used(this can be achieved by hystirx)
	Decomposition	= Breaking up monolithic to microservices.
	
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
=========================================================================================================================================================
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
=================================================================================================

=================================================================================================================================================================
https://www.youtube.com/watch?v=9NVu17LjPSA
https://www.javainuse.com/spring/boot-transaction-isolation
Transaction: It is group of data base commands which are treated as single unit i.e which are executed simultaeously.

A successfull trasnaction should pass above properties. https://www.youtube.com/watch?v=VLc4ewu6lUI
Every transaction should follow ACID properties. ( Automicity, Consistencty, Isolation, Durabiliy)
			Automicity =  All transactions should be success or failed, then it is callled automicity
			Consistencty = Data which are there in table should be in consistent way i.e there should not be any mismatch in data.Then.......
			Isolation = All the transactions to a database should be isolated 
						i.e, Any transaction shouldn't get locked by bcz of 
						other transaction in progress to taht particular row.
			Durability = For any kind of system or hardware or software errors, executed transaction should be  automatically 
						roolled back once the system resumed back.So in this way we can say performacne, durability etc..
			
Types of transactions : 1)Local Transactions 2) Global Transaction
	1) Local Transactions -> If all transactions performed are happeingn on single database then it is Local Transactions
	2) Global Transactions -> If all transactions performed are happeingn on multiple databases then it is Global Transactions.
	
https://youtu.be/D3XhDu--uoI?si=pWFsHeGmg2nJna3e Clear explaintion on the transaction management

Shared Lock  = It will get applied for read operation. Another read operation can still access shared lock. Exclusive Lock cannot access it , mean write not allowed, since it has shared read lock.
Exclusive Lock = It will get applied for write operation.Another read operation (shared lock) cannot access Exclusive Lock write opration. Exclusive Lock cannot access it , mean write not allowed, since it has exclusive Lock.

Concurrency Transactions problems:
    Dirty read			: read the uncommitted change of a concurrent transaction (Write 20, Read 20, Write 30)
							t1-> Update a row of data row 1 from id = 2 from 1
							t2-> Read the data of row 1, he will get id as 2.
							t1-> Due to some failure in trans then updated id will be rolled back.
							So here, t2 transaction is dirtyready transaction which got the uncommitted changes.
							https://youtu.be/5ZEchu2WnD4?list=PL08903FB7ACA1C2FB

	Lost Update			:  T1 is trying to ordering one Iphone and t2 is also ordering 2 Iphones (total 3 iphone). At the end , in DB , total phone should be 7 but we have 9
							bcz t1 silently overwrites t2. 
							(T1 transa is got slow down due to internet issues and t2 trans is very speed due to good internet then updated DB as 8.
								t1 transa is completed and udpated as 9 which has overridden 8).
							https://youtu.be/jD0c4X0tSc8?list=PL08903FB7ACA1C2FB
	Nonrepeatable read	: get different value on re-read of a row if a concurrent transaction updates the same row and commits (Read 20, Write 30, Read 30)
							https://youtu.be/d5QNpsezNTs?list=PL08903FB7ACA1C2FB
							Note: Getting different value for a particular row.
	Phantom read		: get different rows after re-execution of a range query if another transaction adds or removes some rows in the range and commits
							(Read rows 2, insert another row 1 i.e 3, Read rows 3)
							Note: Getting different rows on a particular condition.

These concurrent transaction can be overcome by below methods called Isolation Levels.
	
Transaction Isolation -> To overcome concurrent transactions, we use below isolation levels
	How changes applied by concurrent transactions are visible to each other.
	

	ISOLATION LEVELS:
	@Transactional(isolation = Isolation.READ_UNCOMMITTED)
	DEFAULT				
	READ_UNCOMMITED One transation updated record but not coommited then 2nd trans will get the updateed value before 1st tran commited.
					Allows Dirty Read , Non repeatabel read, Phantom read.

	READ_COMMITED	Blocks the tranaction, so we will use READ_COMMITED_SNAPSHOT isolationl
					SNAPSHOTS		This will use the versionin instead of locking by serilizable.
					It doesn't lock the trans, but other transaction which are in progress will use the previous value until the first transacion is commited.
					t1-> Updating one record of data row 1 , but not commited  (doesnt  lock the transaction)
					t2-> Access the same record of the data row 1, then this will get the prevoious value of that row.
					t1-> commit the t1, then data row 1 will be udpated with new value
					t2-> access the same record, now it will get the udpated value.
					READ_COMMITED_SNAPSHOTS ISLOCATIONl -> No conflits on updating the records,
	
	REPEATABLE_READ
	SERILIZABLE		It locks the transaction until it is commited and other transaction weill be in progress
					t1-> Updating one record of data row 1 , but not commited  (lock the transaction)
					t2-> Access the same record of the data row 1, then this will be in porgress, it cannot access until the above t1 is comited.
					t1-> commit the t1, then atomicatally t2 which is progress will complete the trasaction with udpated value.
					

ISOLATION LEVEL   	Locking Strategy
Read Uncommitted 	Read: No Lock acquired
				  	Write: No Lock acquired
Read Committed 		Read: Shared Lock acquired and Released as soon as Read is done
					Write: Exclusive Lock acquired and keep till the end of the transaction
Repeatable Read 	Read: Shared Lock acquired and Released only at the end of the Transaction
					Write: Exclusive Lock acquired and Released only at the end of the Transaction
Serializable		Same as Repeatable Read Locking Strategy + apply Range Lock and lock is release only at the end of the Transaction.



Optimistic Locking = This can acived using ReadUnCommited and ReadCommited. It will be using version based system while doing transactions.
					T1 = Reading the row1  SharedLock is appeared and released (Version 1)
					T2 = Reading the row1  SharedLock is appeared and released. (Version 1)

					T1 = Updating the record row1 = Absolute lock is acquired. Till end of the transaction (Version 2)
					T2 = Update the same record row1 = No, we cannot , since Absolute lock is already acquired, you cannot apply new one.

					T1 = is completed.
					T2 = It will get the Absolute lock and but failued due to version is different and rolledback.

Pesismistic Locking = This can acived using Repeatable Read and Serializable. It will be using locking the row for the entire transaction. 
					T1 = Reading the row1  SharedLock is appeared and till end of txn
					T2 = Reading the row1  SharedLock is appeared and till end of txn

					T1 = updating the row1  Absolute lock is appeared and till end of txn
					T2 = updating the row1  Cannot lock until above is release.

					

=========================================================================================================================================
https://www.javainuse.com/spring/boot-transaction-propagation
Transaction Propogation
@Transactional{propagation=Propagation.REQUIRED}
Required Propogation-> Two services calling a method which has transaction annotation , then same transaction can be used for both the services.
					@Transaction	
					mehthod1()  will call method2
					@Transaction	
					mehthod2()
					Service1 will call method1 that will call method2 (so here one trasaction will be used for both the methods even it  had @Transaction annotation for the both the methods)
					Service2 will call method2 (so here onew transaction will be created)
@Transactional{propagation=Propagation.SUPPORT}
					If amy transction exisits , then it will make use of it 
					If amy transction not exisits , then it wont create any transactions.
@Transactional{propagation=Propagation.NOT_SUPPORT}
					If amy transction exisits , then it will not make use of it 
					If amy transction not exisits , then it wont create any transactions.
@Transactional{propagation=Propagation.REQUIRES_NEW}
					If amy transction exisits , then it will not make use of it but create new 
					If amy transction not exisits , then it will create new transactions.
@Transactional{propagation=Propagation.NEVER}
					If amy transction exisits , then it will throw the exception
					If amy transction not exisits , then it doesnt create new transactions.
@Transactional{propagation=Propagation.MANDATORY}
					If amy transction exisits , then it will make use of the existing transaction
					If amy transction not exisits , then it will throw the exception.
=================================================================================================================================================================
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
===============================================================================================================================================================					
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

		