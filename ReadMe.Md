# KUBERNETES
    
    Officical Definition - Open Source Container Orchestration Tool
                         - Originally Developed by google
                         - helps us manage containerised applications in different deployment environments
                         
    The need for a container Orchestration tool
                         - trend from monolith to microservices
                         - increased usage of containers
                         - demand for a proper way of managing hundreds of containers
                         
    Features orchestration tools offer
                         - High Availability or no downtime
                         - scalability or high performance (loads fast and users will have high response time from the application)
                         - Disaster recovery - backup and restore (if server explodes or something happens to the service center)
                         
# REFERENCES
   
    Nodes:   https://kubernetes.io/docs/concepts/architecture/nodes/
    Pods:    https://kubernetes.io/docs/concepts/workloads/pods/
    Service: https://kubernetes.io/docs/concepts/services-networking/service/
    Ingres:  https://kubernetes.io/docs/concepts/services-networking/ingress/
    ConfigMap: https://kubernetes.io/docs/concepts/configuration/configmap/
    Secret:  https://kubernetes.io/docs/concepts/configuration/secret/
    Volumes: https://kubernetes.io/docs/concepts/storage/volumes/
    Deployments: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
    StatefulSets: https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/
    Replicaset:https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/
    Deamonset:https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/
    Jobs:https://kubernetes.io/docs/concepts/workloads/controllers/job/
    
    
    
                         
# Kubernetes Components

  **Nodes**
        
        A node could be a virtual or physical machine, depending on the cluster
  
  **Pods:**
  
        Pods are the smallest deployable units of computing that you can create and manage in Kubernetes. its abstraction over container. it creates a running environment on top of container. pod is meant to run one application inside of it.
      
    Kubernetes provides out of the box virtual network. i.e each pod gets its own ip address not the container.
    pods are ephemaral - they can die easily. new ip address on re-creation of a pod thats why services are used 
    
  **Services**
    
    is basically a static(permanent) ip address that is attached to each pod.
    lifecycle of service and pod are not connected. even if pod dies, service and its ip address will stay. 
    External Service - is a service that opens communication to external sources.
    Internal service - is the type of sertvice we mention when creating.
    It is load balanced.
    
  **Ingress**
    
    Instead of service, request first goes to ingress and ingress does the forwarding to the service.
    Database URL we mention usually in the built application. if end point is changed to other db then we need to make changes in application.
    to avoid this complexity we have configMap.
  
  ** External Configuration using COnfigMap and Secrets**
  
    ConfigMap:
    is External configuration to our application.it usually contains configuraiton data like URL to database or some other service that we use.
    if we change the name of the service say from mongo-db-service to mongo-db then we just change it in configMap.
    
    Secrets:
    Even thoug username and password are external configurations, we dont put credentials in configMap.
    putting credentials inside configmap in a plain text format will be insecure.
    Secret is similar to configmap but it is used to store secret data (like username and password)
    it is stored in base64 encoded format.
    
  **Data Persistance - Volumes**
  
    if pod gets restarted, data will be gone. thats problamatic and inconvenient.
    database data needs to be persisted on long term. thats why we use another component of kubernetes called as volumes.
    volumes basically attaches a physical storage on a hard drive to our pod. it could be local or remote.
    k8s doesnt manage data persistance.
    
  **Deployment**
  
    Instead of relying on one application pod and one database pod, we are. replicating everything on multiple servers.
    we will have multiple nodes.
    it is bluepring for my-app pods.
    we can scale up or scale down number of replicas we need.
    database cant be replicated using deployment as database has a state.
    deployments are for stateless apps.
  
  **StatefulSet**
    
    this is meant for stateful apps like databases such as mysql, mongodb.
    It is used for deployment of stateful apps.
    Deployig database application using stateful sets is not easy. 
    Its common practice to host databse applications outside k8s cluster and have just deployments that can scale up or down inside kubernetes cluster
    
# Kubernetes Architecture
        
 **MasterNode**
    
    - Managing processes are done by master nodes.
    - Scheduler
        - watches for new workloads/pods and assigns them to a node based on several scheduling factors
        - checks nodes if healthy?, enough resouces? port available?
        -affinity and anti affinity rules etc
    - api server -for communication 
        - front end to kubernetes plane
        -All communications go through API server (both external and internal)
        - exposes port 443
        -authentication and authorization checks are performed
        //command kubectl apply -f <yaml>
    -cluster store
      - stores configuration and state of the entire application
      -etcd 
            > etcd is the cluster brain
            > Cluster changes get stored in the key values store
            >  what resources are available, did the cluser state change, is cluster healthy etc state info is stored in etcd.
            > (application Data is not stored in etcd).
      -Distributed key value data store
      -single sourde of truth
       //command kubectl apply -f <yaml> //yaml file goes all the way into cluster store
       https://etcd.io/
       https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/
      
    - controller manager
        - Deamon that manages control loop
        - detects cluster state changes. 
        -controller of controllers
        -node controller
            if desired state =1 node, if node fails then node controller brings another node.
         - watches API server for changes and goal is to check if 
            desired state== current state else take action to make 
            desired state== current state
    -cloud controller manager 
    //kudectl apply -f ingress.yaml
    gives load balancer.
    creates load balancer based on underlying cloud controller provider (aws or azure or google cloud)

**Worker Node**

    - provides running environment for our applications
    - communication between worker nodes is via services (Load Balancer)
      -3 Main components (kublet, container runtime and kube proxy)
      - KUBELET
          - interacts with both container and node.
          - starts the pod with container inside.
          - Main agent that runs on every node
          - recieves pod definitions from api server
          - reports node and pod state to master
          - interacts with container runtime to run containers associated with the pod
      -CONTAINER RUNTIME
          -responsible for pulling images, starting and stopping containers
          -CRI (container Runtime Interface)
            - Interface for 3rd party container runtime
          -Containerd
            - https://containerd.io/
      -KUBE PROXY
          - responsible for forwarding requests from services to pods.
          - has intelligent forwarding logic inside which makes sure communication works in a performant way with low overhead. 
          - agent runs on everyone through DeamonSets
          - Responsible for
              - local cluster networking
              -each node gets own unique IP address
              -routing network traffic to load balanced services
    
 # Test/ Local Cluster Set UP
 
   **Minikube** 
   
        - creates virtual box on our laptop
        - node runs in that virtual box
        - 1 Node k8s cluster for testing purpose
        
   **Kubectl**
   
        - command line tool for kubernetes cluster
        - API server is the main entry point for kubernetes cluster. kubectl is the CLI for API server.
        - kubectl is for any type of cluster set up (minikube or cloud)
        
   ** Install Commands**
   
        $ brew update
        ### VM for minikube
        $ brew install hyperkit ### requires xcode 9.0+
        $ brew install minikube minikube has kubectl as dependency so kubectl will get installed along with minikube
       
       Check installation
        $ kubectl version
        $ minikube version      
        $ minikube start --memory-4g // to start minikube with 4g memory allocation
        $ minikube start --vm-driver=hyperkit //to start minikube in vm
        ### Get status of nodes
        $ kubectl get nodes
        $ minikube status
        
  **CRUD Commands**
  
        Create deployment - $ kubectl create deployment [name]
                            example: kubectl create deployment nginx-depl --image=nginx
        
        Edit deployment - $ kubectl edit deployment [name]
                            example:
        
        Delete deployment - $ kubectl delete deployment [name]
                            example:
        
        Note: pods are the smallest unit in k8s but we are not creating pods directly. 
        we create deployments which are abstractionn over the pods.
        
                            
   **Status of different k8s components**
  
        kubectl get nodes|pod|services|replicaset|deployment
        
   **Debugging Tools**
  
        Log to consolde - kubectl logs [pod name]
        get Interactive Terminal - kubectl exec -it [pod name] -- bin/bash
        
   **Create/Edit deployment Commands**
    
    ### create deployment
    $ kubectl create deployment nginx-depl --image=nginx
    $ kubectl get deployment
    $ kubectl get pods
    $ kubectl get replicaset
    
    deployment has blueprint for creating pods
    rest defaults
    pod name is made of replicaset id and pod id
    example: 
    $ kubectl get po
    nginx-depl-5ddc44dd46-qhjpm 
    $ kubectl get replicaset
    nginx-depl-5ddc44dd46
    pod id= qhjpm and replicaset id = nginx-depl-5ddc44dd46
    
    we wont be creating or updating or managing replicasets. we will be working on deployments directly
    Deployment manages a Replicaset which inturn manages a pod which is an abstraction of a container
        
    ### Edit Deployment
    $ kubectl edit deployment nginx-depl
    ### scroll to 
    spec:
      containers:
      - image: nginx
      
     ### change it to 
      spec:
      containers:
      - image: nginx:1.16
      ### we should see
      deployment.apps/nginx-depl edited
      
      $ kubectl get po ### should give new pod details with nginx:1.16
      $ kubectl get replicaset 
      should give 
      NAME                    DESIRED   CURRENT   READY   AGE
        nginx-depl-5ddc44dd46   0         0         0       74m
        nginx-depl-7d459cf5c8   1         1         1       4m10s
      nginx-depl-5ddc44dd46 is old replicaset with no pods
      nginx-depl-7d459cf5c8 is the new replicaset created after we edited
      
   **Kubectl debugging - Logs***
 
       $ kubectl get deployment
       $ kubectl get po
       $ kubectl logs [pod name] example: kubectl logs nginx-depl-7d459cf5c8-jj25f
       
       ### create a mongodb deployment
       $ kubectl create deployment mongo-depl --image=mongo
       $ kubectl get pod
                                                  
        NAME                          READY   STATUS              RESTARTS   AGE
        mongo-depl-85ddc6d66-wxmm9    0/1     ContainerCreating   0          40s
        nginx-depl-7d459cf5c8-jj25f   1/1     Running             0          8m47s

        $ kubectl logs mongo-depl-85ddc6d66-wxmm9
        $ kubectl get pod
        $ kubectl describe pod mongo-depl-85ddc6d66-wxmm9

  **kubectl debugging - exec**
  
        $ kubectl get pod                       
            NAME                          READY   STATUS    RESTARTS   AGE
            mongo-depl-85ddc6d66-wxmm9    1/1     Running   0          5m52s
            nginx-depl-7d459cf5c8-jj25f   1/1     Running   0          13m

        $ kkubectl exec -it mongo-depl-85ddc6d66-wxmm9 -- bin/bash
        root@mongo-depl-85ddc6d66-wxmm9:/# exit
        
        $ kubectl get deployment
             NAME         READY   UP-TO-DATE   AVAILABLE   AGE
                mongo-depl   1/1     1            1           8m
                nginx-depl   1/1     1            1           86m
                
**deleting deployments**

        $ kubectl get pod
        $ kubectl delete deployment mongo-depl
        deployment.apps "mongo-depl" deleted
        $ kubectl get pod
        $ kubectl get replicaset
        
**creating a deployment file**       

        $ touch nginx-deployment.yaml
        
        apiVersion: apps/v1
            kind: Deployment
            metadata:
              name: nginx-deployment
              labels:
                app: nginx
            spec:
              replicas: 1
              selector:
                matchLabels:
                  app: nginx
              template:
                metadata:
                  labels:
                    app: nginx
                spec:
                  containers:
                  - name: nginx
                    image: nginx:1.16
                    ports:
                    - containerPort: 80


        $ kubectl apply -f nginx-deployment.yaml
        deployment.apps/nginx-deployment created
        $ kubectl get pod
        
        change replicas to 2 and
        $ kubectl apply -f nginx-deployment.yaml
        $ kubectl get pod ### should list two pods
        
# K8S YAML Configuration File
  
   **3 parts of configuration file**
        
        1. Metadata
        2. Specification
        3. status (it will be automatically generated and added by kubernetes)
            > kubernetes will compare desired vs actual and if they do not match then it wil fix it
            > this is the basis of the self healing process
            > k8s gets status data from etcd
            
         Yaml online validators to check if yaml file syntax is correct
         
        
        
        nginx-deployment.yaml
        apiVersion: apps/vl
        kind: Deployment
        metadata:
            name: nginx-deployment
            labels: -
        spec:
           replicas: 2
           selector: -
           template: -
           
        nginx-service.yaml   
        apiVersion: v1
        kind: Service
        metadata:
            name: nginx-deployment
        spec:
           selector: -
           ports: - 
           
        
        connecting deployments to service to pods
        Demo
        
        
  
  
  
  
  
  
  
  
  
  
                         
                         
