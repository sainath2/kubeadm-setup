## Kubeadm Cluster Setup Scripts


cd kubeadm-setup/

cd scripts

run ./common.sh in all master and worker nodes

run ./master.sh in only master node ( we will join token in master ) 

then run this in all worker nodes ->

 kubeadm join 54.209.223.35:6443 --token kmmgbc.f4y8m4mtc6364lli \
--discovery-token-ca-cert-hash sha256:292d1d8f221c5068328330f4e80382515a268610ed4dc0c734fbcb68ffd5f117

now come back to master nodes -> then run below commands
##############################


cd kubeadm-setup/manifests  -> we can see "metrics-server.yaml" 

kubectl apply -f metrics-server.yaml ( now we can see kubectl top nodes , this metrics-server will create metrics for nodes and pods)

Deploy sample nginx app and expose with nodeport

cd kubeadm-setup/manifests -> we can see "sample-app.yaml" -> kubectl apply -f sample-app.yaml

now the nginx deployment with two replicas running on two worker nodes 

NOTE: 1. WE SHOULD RUN DEPLOYMENTS FROM MANAGEMENT SERVER ONLY 
      2. WE WILL HAVE .KUBE/CONFIG FILE IN ONLY MASTER/MANAGEMENT SERVER ONLY , WE WILL NOT HAVE .KUBE/CONFIG FILE IN WORKER NODES 
	  3. WE CAN DO KUBERNETES OPERATIONS IN MANAGEMENT/MASTER NODE ONLY, APPS WILL NOT RUN IN MANAGEMENT/MASTER NODE , OUR APPS RUN ONLY ON WORKER NODES 
	  
now on master -> kubectl get pods -o wide ( we can see pods are running in nodes with details )

              -> now kubectl get svc ( we can see svc with nodeport and clusterIP)
			
eg: root@ip-172-31-87-250:~/kubeadm-scripts/manifests# kubectl get svc

NAME            TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
nginx-service   NodePort    10.99.72.70   <none>        80:32000/TCP   44m

testing with CLUSTER-IP
---------------------------

now copy CLUSTER-IP(10.99.72.70)and port(80) login to worker nodes 1 -> curl 10.99.72.70:80 ( we will get nginx output)

											login to worker nodes 2 -> curl 10.99.72.70:80 ( we will get nginx output)
											
											

testing with NodePort with two ways 
------------------------------------
1.login to workernode -> copy workernode publicip:32000 -> http://52.91.97.228:32000/  -> we will get nginx output
2. goto browser -> copy workernode publicip:32000 -> http://52.55.160.175:32000/ -> we will get nginx output



