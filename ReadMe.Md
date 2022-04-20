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
      -Distributed key value data store
      -single sourde of truth
       //command kubectl apply -f <yaml> //yaml file goes all the way into cluster store
       https://etcd.io/
       https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/
      
    - controller manager
        - Deamon that manages control loop
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
      -3 Main components (kublet, container runtime and kube proxy)
      - KUBELET
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
          - agent runs on everyone through DeamonSets
          - Responsible for
              - local cluster networking
              -each node gets own unique IP address
              -routing network traffic to load balanced services
    
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
                         
                         
