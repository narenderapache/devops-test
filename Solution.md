# Solution
This solution is developed using below technology stack
  * Docker to containerse the node.js application
  * Dockerhub to store the images
  * GKE - Managed Kubernetes platform on Google Cloud
  * Circleci - Continuous Integration & delivery
    * This pipeline triggers automatically when the branch is pused to Github
    * Three jobs are created for end to end deployment
      * Run Unit Tests
      * Build and Push Docker image
      * Deploy Kubernetes deployment and Service on GKE
  * Kubernetes manifests for Deployment and Service

* Target environment consists of:
  * GKE service (load-balancer) accessible via HTTP on port 80.	
  * Deployment (with 2 pods) is accessible via HTTP on port 3000.

# Below tests ensure that application is working
  * Get pods
  ```
  kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
build-deploy-869856d6f5-5d9m4   1/1     Running   0          80m
build-deploy-869856d6f5-nl82f   1/1     Running   0          80m
  ```
  * Get Service
  ```
  kubectl get svc
  NAME              TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE
  buildit-service   LoadBalancer   10.3.253.255   34.105.25.206   80:32436/TCP   93m
  kubernetes        ClusterIP      10.3.240.1     <none>          443/TCP        5h41m
  ```
  * Access the load-balancer
  ```
  curl -v http://34.105.25.206
  * Rebuilt URL to: http://34.105.25.206/
  *   Trying 34.105.25.206...
  * TCP_NODELAY set
  * Connected to 34.105.25.206 (34.105.25.206) port 80 (#0)
  > GET / HTTP/1.1
  > Host: 34.105.25.206
  > User-Agent: curl/7.54.0
  > Accept: */*
  >
  < HTTP/1.1 200 OK
  < Date: Sat, 26 Sep 2020 21:15:04 GMT
  < Connection: keep-alive
  < Keep-Alive: timeout=5
  < Transfer-Encoding: chunked
  <
  * Connection #0 to host 34.105.25.206 left intact
  Hi there! I'm being served from build-deploy-869856d6f5-nl82f%
  ```

 ## Out of scope	
  * Google Cloud Kubernetes platform (Created manually)
  * External DNS for the load balancer - Not used because of the cost involved
  * Helm chart is not used for this solution
