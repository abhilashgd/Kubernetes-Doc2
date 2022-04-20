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
    
    
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
                         
                         
