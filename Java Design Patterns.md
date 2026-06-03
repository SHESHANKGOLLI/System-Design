Design Patterns:
Ex:  We are suffering from fever, we directly go to medical store instead of doctor but still we don’t see fever is coming normal, after few scolding from parents we go to doctor and get the prescritption frm doctor ,so and so medicine with so and so anti-biotic (now fever got down atumaticcaly)
Here antibiotech is main (so design patterns will work as anti biotech)

GOF(Gang Of Four) = This pattern is implemented  in oop languages
JAVA EE Design Pattern = This pattern is implemented  in java ee only


Product Catalog:
Now we need a product catalog for GOF pattern:
Ex: restaurant menu has categorized list (starters, main course, desserts etc)
a)Creational Patterns (Solution related to objects creation)
	SingleTon Pattern,	
	FactoryMethod – Abstract plan class , subclasses DomesticPan, CommericialPlan, IndustrialPlan
	BuilderDesign Pattern
	Composite Design Pattern
	ProtoType
b)Structural Patterns (Solutions related to complex objs creation)
	Proxy Design Pattern 
				A proxy hides the complexity involved in communicating with the real object. 
				@Transaction background somehting to rn
	Decorator Design Pattern
				TaskDecorator which is used for ThreaedPoolExecutor
	Adapter Design Pattern = Converts the interface of a class into another interface that a client wants. MapStruct mapper interfaces (Converting between two objects)
	Facade Design Pattern =  MobileShop, ShopKeeper,Iphone,BlackBerry, ShopKeeper, Client
							describes a higher-level interface that makes the sub-system easier to use.
							https://www.javatpoint.com/facade-pattern

c)Behavioural Patterns (Solutions related to interaction b/w classes objects)
	Chain of Responsibility = Response of each service is interlinked to each other.
	Template Design Pattern	= 	https://www.digitalocean.com/community/tutorials/template-method-design-pattern-in-java
	Strategy Design Patter

a)Presentation tier
b)Business Tier
c)Integration Tier
==========================================================================================================================================================
Spring Design patterns
Single Ton design patter, factory method design pattern, 

========================================
Solid Principles
DRY (Don's Repeat Yourself ) and KISS (Keep It Simple Stupid)Principal = https://dzone.com/articles/software-design-principles-dry-and-kiss
YAGNI (You Aren't Gonna Need It): https://workat.tech/machine-coding/tutorial/software-design-principles-dry-yagni-eytrxfhz1fla
TDD (Test Driven Development)
BDD Cucumber
=====================================================================

Strategy Design Pattern :

The Strategy Design Pattern is a behavioral design pattern that allows you to define a family of algorithms, encapsulate each one in a separate class, and make them completely interchangeable at runtime.

1. You will have one interface  
2. Two or three implementation classes which implements above interface and override the method.
3. The Context Class Now new class, will have one setter method where it set one design pattern implementation class.
4. With that class, it will call the actual overrided method.
5. In the main class of client code, we will create an object of contextual class and then assign one object of implementation class.

interface Strategy {
	void StrategyMethod();
}

class StrategyImp1 implements Strategy {
	void StrategyMethod(){
	System.out.println("StrategyImp1 method executed")
	}
}

class StrategyImp2 implements Strategy {
	void StrategyMethod(){
	System.out.println("StrategyImp2 method executed")
	}
}

class ContextStrategy {
	Strategy strategy;

	public void setStrategy(Strategy strategy)
	{
	this.strategy =. strategy;
	}
	public void doSomething()
	{
		strategy.StrategyMethod();
	}

}
class MainStrategy {
	main(){
		ContextStrategy contextStrategy = new ContextStrategy();
		contextStrategy.setStrategy(new StrategyImp1());
		contextStrategy.doSomething();
	}
}

public interface RouteStrategy
void buildRoute(String source, String destination);

public class DrivingStrategy implements RouteStrategy {
public class WalkingStrategy implements RouteStrategy {

public class NavigatorContext {
    private RouteStrategy routeStrategy;

public class Main {
    public static void main(String[] args) {
        NavigatorContext navigator = new NavigatorContext(); String startPoint = "Central Station"; String endPoint = "Airport";

        // User chooses to drive
        navigator.setRouteStrategy(new DrivingStrategy());
        navigator.executeRoute(startPoint, endPoint);
===================================================================================================================================================
Template Design Pattern:

The Template Method Design Pattern is a behavioral design pattern that defines the skeleton of an algorithm in a superclass but lets subclasses override specific steps of the algorithm without changing its overall structure.

1. You will have one abstract class with few concrete methods already implemented and few abstrac methods needs implementation.
2. So new abstrac class will implement above abstract class and implement those abstract methods.
3. Now main class will create an object of either of one implementation class and calls the methods.
	It can call already concrete methods and overrided method from the implementation classes

public abstract class BuiltHouse {
	public final void built(){
	pillars();
	foundation();
	walls();
	rooms();
	}

	public void pillars() {
	System.out.println("pillars are done")
	}

	public void foundation() {
	System.out.println("foundation is done")
	}

	protected void walls();
	protected void rooms();

}

class WoodenHouse extends BuiltHouse {
	void walls() {
	System.out.println("wooden walls is done")
	}

	void rooms() {
	System.out.println("wooden rooms is done")
	}
}

class ConcreteHouse extends BuiltHouse {
	void walls() {
	System.out.println("Concrete walls is done")
	}

	void rooms() {
	System.out.println("Concrete rooms is done")
	}
}

class MainClass {
main(){
BuiltHouse builtHouse = new WoodenHouse();
builtHouse.built();
}
}

===================================================================================================================================================

The Decorator Design Pattern is a structural design pattern that allows you to dynamically add new behaviors or responsibilities to an object without altering its source code or affecting the behavior of other objects from the same class.

public interface TaskDecorator {
	Runnable execute(Runnable runnable);
}

public class MdcDecorator implements TaskDecorator {
	@Override
	public Runnable execute(Runnable runnable) {

		Map<String, String> contextMap = MDC.getCopyofContextMap();

		() -> {
			try {
				if (contextMap != null)
				{
					MDC.setContextMap(contextMap)
				}
				runnable.run();
			}
			finally {
				MDC.clear();
			}
		};

	}
}

@Configuration
public class AsyncConfig {

	@Bean(""AsyncTaskExecutor)
	public Executor threadExecutor() {
	ThreadPoolTaskExecutor executer = new ThreadPoolTaskExecutor();
	executer.setCorePoolSize();
	executer.setMaxPoolSize();
	executer.setQueueCapacit();
	executor.setTaskDecorator(new MdcDecorator());
	}
}

====================================================================================================

Adapter, Facade, Proxy