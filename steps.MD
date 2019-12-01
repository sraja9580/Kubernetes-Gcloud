1. Create Google Cloud Account
------------------------------

2. Install GCloud
-----------------
	https://cloud.google.com/sdk/install
	gcloud auth login (in command window to login to cloud)
	
3. Intall Kubectl
-----------------
	https://kubernetes.io/docs/tasks/tools/install-kubectl/
	
	F:\SW\kubectl.exe
	

4. Hello-world-rest-api
-----------------------
	1. Load the project to STS
	2. Run and Test it in local
		http://localhost:8081/hello-world
		
	3. MVN clean build (Docker Image also gets created)
		mvn clean package
		
	4. Run the container in local and test
		docker images (check local images)
		docker run -p 4000:4000 raja9580/hello-world-rest-api:0.0.1.RELEASE	
		http://192.168.99.100:4000/hello-world (local host will not work with docker toolbox)
		
	5. push the docker image to docker hub
		docker login  (it will prompt provide username and pwd)
		docker push raja9580/hello-world-rest-api:0.0.1.RELEASE
		
	6. Create Deployment and Expose using kubectl (using powershell)
	
		*Get the login string from cloud and use it in powershell
		
		kubectl create deployment hello-world-rest-api --image raja9580/hello-world-rest-api:0.0.1.RELEASE	 
		kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080
		
		http://35.222.123.108:8080/hello-world-bean  (IP and port will change)
		
		Future  deployment of same application
		kubectl set image deployment hello-world-rest-api hello-world-rest-api=raja9580/hello-world-rest-api:0.0.1.RELEASE	
		
	7.get yaml file for deployment and services

		kubectl get deployment hello-world-rest-api -o yaml > deployment.yaml
		kubectl get service hello-world-rest-api -o yaml > service.yaml
		
	8. Change few things like no of pods and app the change
		kubectl apply -f .\deployment.yaml
		
	9.Cleanup Yaml file.Delete app from cluster and deploy app using yaml file
	
		kubectl delete all -l app=hello-world-rest-api
			* -l stands for label
		kubectl apply -f .\deployment.yaml
		* now app and service should be up and running :-)
			
			
5. Playing with YAML file
-------------------------

	Reducing Down time with minReadySeconds *TEMP* fix
	--------------------------------------------------
	1. call the service in curl in cloud shell(As windows dont support curl)
		watch -n 0.1 curl 35.222.123.108:8080/hello-world
		
	2. Deploy new version 
		*during deployment you will see lot of request still goes to new version which is not ready and fails
		 kubectl apply -f .\backup\deployment-01-after-cleanup.yaml
		
	3. Set *******minReadySeconds******** in Deployment spec section
		apiVersion: extensions/v1beta1
		kind: Deployment
		metadata:
		  labels:
			app: hello-world-rest-api
		  name: hello-world-rest-api
		  namespace: default
		spec:
		  replicas: 2   
		  #Below will make sure for specified no of seconds request will go to old pods
		  minReadySeconds: 120
		  
	4. Deploy new version 
		*during deployment you will see request still goes to old version for 120sec ,that give time for new version to get ready
		 kubectl apply -f .\backup\deployment-01-after-cleanup.yaml
		
		
	Running app with just ReplicaSet ,without deployment
	----------------------------------------------------
	*ReplicaSet  will not care if even if you update the app, it just make sure specified no of pods are always running.
	*ie why we need deployment,Let us see this in action 
	
	1. Delete existing app version if running
	
		kubectl delete all -l app=hello-world-rest-api
			* -l stands for label
			
	2. run app by applying below which contains replica set 
		
		kubectl apply -f .\backup\deployment-02-using-replica-set.yaml
		* You will see replica-set and service alone created no deployment
		* Check the app it will be running
		http://35.194.49.55:8080/hello-world
		
	3. call the service in curl in cloud shell(As windows dont support curl)
		watch -n 0.1 curl 35.194.49.55:8080/hello-world
	
	4. Deploy new version 
		*you will see replica-set config updated but, check curl request still goes to old instance
		 kubectl apply -f .\backup\deployment-02-using-replica-set.yaml
		 
	5. Check List of PODS	
	
		kubectl get pods		
		You will see only old pods ,no new pods created
		*ReplicaSet only make sure specified no of pods are running it doesn't care about new version
		
		kubectl delete pod hello-world-rest-api-fs7w5
		*If you delete old pods one by one it will create new pods with updated config
		 Now check the curl it will have new version
		 
	Running app with just ReplicaSet ,without deployment
	----------------------------------------------------