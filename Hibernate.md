Hibernate State
Persistent, Transient, Detached.


=======
Session object holds the first level cache data. It is enabled by default
Second level cache is hold by sessionFactory


Cache:
EH (Easy Hibernate) Cache
OS Cache
Swarm Cache
JBoss Cache

strategies 
Implementation	read-only	nonstrict-read-write	read-write	transactional
Yes yes yes no
yes yes yes no
yes yes no no 
no no no yes

   Session session1=factory.openSession();    
    Employee emp1=(Employee)session1.load(Employee.class,121);    
    System.out.println(emp1.getId()+" "+emp1.getName()+" "+emp1.getSalary());    
    session1.close();    
        
    Session session2=factory.openSession();    
    Employee emp2=(Employee)session2.load(Employee.class,121);    
    System.out.println(emp2.getId()+" "+emp2.getName()+" "+emp2.getSalary());    
    session2.close();  

As we can see here, hibernate does not fire query twice. 
If you don't use second level cache, hibernate will fire query twice because both query uses different session objects.
======================================================================================================================
Hibernate Caching
https://javabeat.net/introduction-to-hibernate-caching/

example:
https://www.digitalocean.com/community/tutorials/hibernate-caching-first-level-cache  