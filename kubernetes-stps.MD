1. Create Google Cloud Account
Cluster: 
        Combination of Work Nodes and Master Nodes

2. K8S Kubernetes

3. Cloud Shell
     kubectl  ---> Command to interact with cluster.It is like CF
	 
https://github.com/in28minutes/kubernetes-crash-course#commands-executed-during-the-course

4. How to connect to Kubernetes Engine in google cloud
------------------------------------------------------
   You can get the connection URL from cluster.Below is taken from cluster it might change based on project namd and other things
   
   gcloud container clusters get-credentials raja-cluster --zone us-central1-a --project propane-facet-260516
   
   kubectl version        ---to check the version
   
Deploy First application from docker hub
----------------------------------------
	https://hub.docker.com/r/in28min/hello-world-rest-api
	kubectl create deployment hello-world-rest-api --image in28min/hello-world-rest-api:0.0.1.RELEASE 
	kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8080
	
	Go check in service section in cluster,u will see application
	You will get the host and port from service section add endpoind and try accessing the service
	http://35.222.123.108:8080/hello-world-bean  (IP and port will change)
	
Getting list of Events:
----------------------
	kubectl get events
	
	kubectl get pods
	kubectl get pods -o wide   (gives more details)
	
	kubectl get replicasets
	
	kubectl get deployments
	kubectl get services
	
Expalin:
-------- 
	kubectl explain pods  (explains what is pod)
	
	kubectl describe pod podName (details of particular pod)
	kubectl describe pod hello-world-rest-api-58ff5dd898-wjgdb
	
Scale
-----
	kubectl scale deployment deployementName --replicas=3
	kubectl scale deployment hello-world-rest-api --replicas=3
	
Update Deployment or Deploying New version of application
---------------------------------------------------------
	kubectl set image deployment deployementName containerName=dockerImage
	kubectl set image deployment hello-world-rest-api hello-world-rest-api=in28min/hello-world-rest-api:0.0.2.RELEASE --record
	
History of Particular Application:
----------------------------------
	kubectl rollout history deployment deployementName
	kubectl rollout history deployment hello-world-rest-api
	
Status of the latest rollout
----------------------------
	kubectl rollout status deployment deployementName
	kubectl rollout status deployment hello-world-rest-api
	
rollback to previous deployment
-------------------------------
	kubectl rollout undo deployment deployementName --to-revision=revisionNo
	kubectl rollout undo deployment hello-world-rest-api --to-revision=1
	
View Logs
---------
	kubectl logs podId
	kubectl logs podId -f
	kubectl logs hello-world-rest-api-58ff5dd898-bgwt4 -f

Get yaml file for deployment and Service
----------------------------------------
	kubectl get deployment deployementName -o yaml  (to read in command line)
	kubectl get deployment deployementName -o yaml > deployment.yaml (to write it in file)
	kubectl get service hello-world-rest-api -o yaml > service.yaml
	
Apploy Changes
--------------
	kubectl apply -f .\deployment.yaml
	