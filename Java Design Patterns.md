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


Template Design Pattern:

The Template Method Design Pattern is a behavioral design pattern that defines the skeleton of an algorithm in a superclass but lets subclasses override specific steps of the algorithm without changing its overall structure.

1. You will have one abstract class with few concrete methods already implemented and few abstrac methods needs implementation.
2. So new abstrac class will implement above abstract class and implement those abstract methods.
3. Now main class will create an object of either of one implementation class and calls the methods.
	It can call already concrete methods and overrided method from the implementation classes

