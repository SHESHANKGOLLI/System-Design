https://www.youtube.com/watch?v=gMmcRbd8L5Y&t=1624s
Cloud:
Kubernetes:
1. Kubernetes is known as Container Orchestration Engine or Containerized Management Tool.
When i say containerzied management tool-> each term has its own definitation.
	a) Container -> Where we run our java application.
	b) Management Tool -> Where it handels deployment, Sheduling, Scaling and Load Balancing the application.

- Components: Continaer -> Pods, Node , Cluster , ReplicaSets ,Service, Deployment, ConfigMap, Secrets, Etcd. 

	a) Pods:
	-So in kubeneters, containers get runs under the pods. A pod can have single or multiple containers in it.
	-Each Pod will have the single ipAdddress.
	
	b) Node:
	- A node contain, single pod or multiple pods.

	c) Cluster:
	- A cluster can have one node or multiple nodes
		i) Cluster contains a one master node and multiple child nodes.(Think about Redis cache cluster).

	What if any Pod die due to some issues, then below Replica Set came into the picture.	
	d) Replica Set/Replicaion Controller
	- At the time of deployment , we will specify the replicas for each pod. (Nothing same data will be there in another pod as a backup)
	- When a pod goes down which have couple of microservices running it at run time due to techinal issues, then replica will be taking care of detaching
this existing pod and attaching the new pod with new IP address.

	So, now when a pod goes down, then replica is helping to add new pod with new Ip address , then how does other ipaddress know about the ipaddress.

	e) Service:
		i) So here, one pod or more pods will be associated to one service.(With label and selector)
		ii) Service will provide some ip and DNS name. So now using DNS name , each pod will interact with other pod. So problem with e will be overcome.
		iii) Service will do the load the balancing and handles the traffic and divert it to the respective pods.
		iv) ServiceType will provide 3 more features
			a) Cluster IP b)Node Port c)Load Balancer.
	f) Deployment:
		i) It used to create a deployment object. In Deployment.xml we will specify all the details inorder to deploy the applciation.
=		ii) kubectl deployment first-deployment <Docker-Image> --port=8080 --replicas=4
	g) Secrets and ConfigMap
		i) This is used to store the password and any sensitive information about the pods.
		ii) This will be store outside of pods. Means Node =  Secerts & ConfigMap and Pod.
		iii) Secret - with encrypte text , ConfigMap -> Plain Text
	i) ETCD:
		i) It is a data base for kubenetes, which store in the form of key , value pair. It stores secetes and configmap data in this ETCD.
		ii) Limit is 1MB , to store the secrets.

===================================================================================
Kubenetes Architecture.
	
Architecture: 
		API Server, Sheduler, Controller Manager, ETCD

	Master Node:
	1) In K8 Architecture-> Each Cluster will have one master node and one child nodes.
	2) Master Node will take the responsibility working of child nodes. Mean if any pod is down, he will take care of adding the replicas. .
	If any node is down, he will assign all the pods to the working node.
	3) Master Node contains below a) API Server b)Shedulers c)Controller Manager d)ETCD.
		
		a) API Server:
		-Which acts a API Gateway or entry point of the application
		-Which check whether all the pods are working or not and get the statistics of all the nodes.
		-We can acheieve this by kubectl commands or the k8 dashboard.
		
		b)Sheduler:
		-Which check the memory size of the pods and health, if memory is full for any of the pods, then sheduler will try to assign new pods.
		-How does shdeuler understand how much cpu or memory is occupied with pods , so here ETCD Database will have those details of pods or cluster
and this will share the information to the sheduler . So sheduler will assign the pods based on the memory.

		c)Controller Manager:
		-It detech whenever any pods crashes,then intimate the shdeuler for replica.
		- Node controller
		- Replication Controller
		- Endpoints controller
		- Service Account and Token Controller.


	Worker Node:
	1) It Contains kubelet, Kube-proxy and Container Runtime.
	2) Kubelet and Kube-proxy are used to communicate with the master node and share the information of the ndoe.

==================================================================================
Mini Kube:
	
		


	

===================================================================================
Amazon Web services - newly called as Cloud computiog
			1) One virtual machine is called as EC2 instace (Elastic Compute Cloud)
			2) EC2 (where we run the application) and S3 (simple storage service) storages (where we add our jar or war files)
			3) Putty (Connect to ec2 and deply project)
			4) AWS lambda = Serverless computing service 
			5) AWS Apigate waY - AWS Elastic beanstalk - spring boot aplciation.