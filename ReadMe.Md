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
    
    
                         
# Kubernetes Components

  **Nodes**
        
        A node could be a virtual or physical machine, depending on the cluster
  
  **Pods:**
  
        Pods are the smallest deployable units of computing that you can create and manage in Kubernetes. its abstraction over container. it creates a running environment on top of container. pod is meant to run one application inside of it.
      
    Kubernetes provides out of the box virtual network. i.e each pod gets its own ip address not the container.
    pods are ephemaral - they can die easily. new ip address on re-creation of a pod thats why services are used 
    
  **Service**
    
    is basically a static(permanent) ip address that is attached to each pod.
    lifecycle of service and pod are not connected. even if pod dies, service and its ip address will stay. 
    External Service - is a service that opens communication to external sources.
    Internal service - is the type of sertvice we mention when creating.
    
  **Ingres**
    
    Instead of service, request first goes to ingress and ingress does the forwarding to the service.
    Database URL we mention usually in the built application. if end point is changed to other db then we need to make changes in application.
    to avoid this complexity we have configMap.
    
  **ConfigMap**
  
    is External configuration to our application.it usually contains configuraiton data like URL to database or some other service that we use.
    if we change the name of the service say from mongo-db-service to mongo-db then we just change it in configMap.
    
  **Secret**
    
    Even thoug username and password are external configurations, we dont put credentials in configMap.
    putting credentials inside configmap in a plain text format will be insecure.
    Secret is similar to configmap but it is used to store secret data (like username and password)
    
    
    
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
                         
                         
