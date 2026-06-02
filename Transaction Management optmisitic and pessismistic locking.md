
Concurrency Transaction Problems:

====================================================================================
https://www.youtube.com/watch?v=9NVu17LjPSA
https://www.javainuse.com/spring/boot-transaction-isolation
Transaction: It is group of data base commands which are treated as single unit i.e which are executed simultaeously.

A successfull trasnaction should pass above properties. https://www.youtube.com/watch?v=VLc4ewu6lUI
Every transaction should follow ACID properties. ( Automicity, Consistencty, Isolation, Durabiliy)

Automicity =  All transactions should be success or failed, then it is callled automicity

Consistencty = Data which are there in table should be in consistent way i.e there should not be any mismatch in data.Then.......
			
Isolation = All the transactions to a database should be isolated 
i.e, Any transaction shouldn't get locked by bcz of  other transaction in progress to taht particular row.

Durability = For any kind of system or hardware or software errors, executed transaction should be  automatically roolled back once the system resumed back.So in this way we can say performacne, durability etc..
			
Types of transactions : 1)Local Transactions 2) Global Transaction
	1) Local Transactions -> If all transactions performed are happeingn on single database then it is Local Transactions
	2) Global Transactions -> If all transactions performed are happeingn on multiple databases then it is Global Transactions.
	
https://youtu.be/D3XhDu--uoI?si=pWFsHeGmg2nJna3e Clear explaintion on the transaction management

Shared Lock  = It will get applied for read operation. Another read operation can still access shared lock. Exclusive Lock cannot access it , mean write not allowed, since it has shared read lock.

Exclusive Lock = It will get applied for write operation.Another read operation (shared lock) cannot access Exclusive Lock write opration. Exclusive Lock cannot access it , mean write not allowed, since it has exclusive Lock.

Concurrency Transactions problems:
    Dirty read: 
    	read the uncommitted change of a concurrent transaction (Write 20, Read 20, Write 30) 
    	t1-> Update a row of data row 1 from id = 2 from 1 
	t2-> Read the data of row 1, he will get id as 2. 
	t1-> Due to some failure in trans then updated id will be rolled back. 
	So here, t2 transaction is dirtyready transaction which got the uncommitted changes.
	
    Nonrepeatable read	: 
    	get different value on re-read of a row if a concurrent transaction updates the same row and commits (Read 20, Write 30, Read 30)
	https://youtu.be/d5QNpsezNTs?list=PL08903FB7ACA1C2FB
        Note: Getting different value for a particular row.
	
    Phantom read : 
    	Get different rows after re-execution of a range query if another transaction adds or removes some rows in the range and commits
	(Read rows 2, insert another row 1 i.e 3, Read rows 3)
	Note: Getting different rows on a particular condition.

These concurrent transaction can be overcome by below methods called Isolation Levels.
	
Transaction Isolation -> To overcome concurrent transactions, we use below isolation levels


ISOLATION LEVEL   	Locking Strategy (Read and Write)
Read Uncommitted 	
	Read: No Lock acquired
	Write: No Lock acquired
Read Committed 		
	Read: Shared Lock acquired and Released as soon as Read is done
	Write: Exclusive Lock acquired and keep till the end of the transaction
Repeatable Read:
	Read: Shared Lock acquired and Released only at the end of the Transaction
	Write: Exclusive Lock acquired and Released only at the end of the Transaction
Serializable:
	Same as Repeatable Read Locking Strategy + apply Range Lock and lock is release only at the end of the Transaction.
	How changes applied by concurrent transactions are visible to each other.
	

ISOLATION LEVELS:
@Transactional(isolation = Isolation.READ_UNCOMMITTED)
	DEFAULT				
	READ_UNCOMMITED 
		One transation updated record but not coommited then 2nd trans will get the updateed value before 1st tran commited.
					Allows Dirty Read , Non repeatabel read, Phantom read.

	READ_COMMITED
		Blocks the tranaction, so we will use READ_COMMITED_SNAPSHOT isolation SNAPSHOTS This will use the versionin instead of locking by serilizable.
		It doesn't lock the trans, but other transaction which are in progress will use the previous value until the first transacion is commited.
	
		t1-> Updating one record of data row 1 , but not commited  (doesnt  lock the transaction)
		t2-> Access the same record of the data row 1, then this will get the prevoious value of that row.
		t1-> commit the t1, then data row 1 will be udpated with new value
		t2-> access the same record, now it will get the udpated value.
		READ_COMMITED_SNAPSHOTS ISLOCATIONl -> No conflits on updating the records,
	
	REPEATABLE_READ:
		t1 -> Both are reading same row -> Each got 1
		t2 -> Both are reading same row -> Shared lock is acuired already, so nothing

		t1 -> Updates the value -> Exclusive lock is acquired till end.
		t2 -> Still get the old value

		t1 is completed.
		t2 will get the updated value.

		By this we can tell that, non_repeatable concurrency txn got resolved


	SERILIZABLE:
		It locks the transaction until it is commited and other transaction weill be in progress
		t1-> Updating one record of data row 1 , but not commited  (lock the transaction)
		t2-> Access the same record of the data row 1, then this will be in porgress, it cannot access until the above t1 is comited.
		t1-> commit the t1, then atomicatally t2 which is progress will complete the trasaction with udpated value.
					

Optimistic Locking:
	This can acived using ReadUnCommited and ReadCommited. It will be using version based system while doing transactions.
	
	T1 = Reading the row1  SharedLock is appeared and released (Version 1)
	T2 = Reading the row1  SharedLock is appeared and released. (Version 1)

	T1 = Updating the record row1 = Exclusive lock is acquired. Till end of the transaction (Version 2)
	T2 = Update the same record row1 = No, we cannot , since Exclusive lock is already acquired, you cannot apply new one.

	T1 = is completed.
	T2 = It will get the Exclusive lock and but failued due to version is different and rolledback.

Pesismistic Locking:
	This can acived using Repeatable Read and Serializable. It will be using locking the row for the entire transaction.

	T1 = Reading the row1  SharedLock is appeared and till end of txn
	T2 = Reading the row1  SharedLock is appeared and till end of txn

	T1 = updating the row1  Exclusive lock is appeared and till end of txn
	T2 = updating the row1  Cannot lock until above is release.
====================================================================================
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
=======================================================================


Isolation Levele:

Read_Uncommited
RepeatibleRead
ReadCommited
Serializabe
