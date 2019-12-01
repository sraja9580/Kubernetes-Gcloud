1.Cluster
---------
        Combination of Work Nodes and Master Nodes

2. K8S Kubernetes
-----------------

3. Cloud Shell
--------------
     kubectl  ---> Command to interact with cluster.It is like CF

4. Pods
-------
	*smallest Deployable unit
	*Container lives inside the pod
	*pods is the collection of containers
	
5.Replicaset
------------
	*Make sure specified number of pods are always up and available
	* You can scale the no of pods using below
		kubectl scale deployment deployementName --replicas=3

6.Deployment
-------------

   Update version:
   ---------------
        Updating new version of docker image to existing deployementName
	
	kubectl set image deployment deployementName containerName=dockerImage	
	kubectl set image deployment hello-world-rest-api hello-world-rest-api=in28min/hello-world-rest-api:0.0.2.RELEASE
	
7. Services
-----------
	
	Master Node
	-----------
		Distributed DataBase(ETCD)
		   *Stores the Desired state of the application
			config change
			scaling 
			Deployment
			
		API Server
		  *interaction with Cluster is done via API server
		   kubectl->cluster
		   cloudUi->
		   
		Scheduler  
			Schedules the pods on appropriate node by considering memory ,cpu etc
			
		Controller Manager
			Makes sure current state of the application is matches to desired state
	
	Worker Node:
	------------
		Pods
		
		Node Agent(Kubelet)
			monitor the node and communicates its to master node
			ex: if the pod in a node goes down it report it to Controller Manager
		  
		Networking Component (Kube-proxy)
			helps in exposing service nodes and pods
		
		Container Runtime
			
	
	