# program9
Working :
1.Prerequisites
https://kind.sigs.k8s.io/docs/user/quick-start/
./kind.exe create cluster
https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/
Move-Item .\kubectl.exe kubernetes\kubectl.exe
2. Docker Image
•	Assuming you have a Docker image, say myapp:1.0, you can either pull it from Docker Hub or build it locally.
docker pull samplemyapp/myapp
3. Create a Kubernetes Pod
•	A Pod is the basic unit of deployment in Kubernetes. Let’s create a Pod using the Docker image.
Pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  labels:
    app: myapp
spec:
  containers:
  - name: myapp-container
    image: myapp:1.0
    ports:
    - containerPort: 80

Apply the Pod configuration:
kubectl apply -f pod.yaml
Verify that the Pod is running:
kubectl get pods

4. Create a Kubernetes Deployment
Deployments provide a way to manage a set of identical Pods. They help with scaling and updating applications.
deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
  labels:
    app: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp-container
        image: myapp:1.0
        ports:
        - containerPort: 80
Apply the Deployment configuration:
kubectl apply -f deployment.yaml
Verify the Deployment and Pods:
kubectl get deployments
kubectl get pods

5. Expose the Deployment
To access your application outside the Kubernetes cluster, you need to expose it via a Service.
service.yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp-service
spec:
  selector:
    app: myapp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
Apply the Service configuration:
kubectl apply -f service.yaml
Verify the Service:
kubectl get services
6. Scaling the Deployment
You can scale your application by adjusting the replicas field in the Deployment configuration.
Scale using kubectl:
kubectl scale --replicas=5 deployment/myapp-deployment
Verify the scaling:
kubectl get deployments
kubectl get pods
7. Updating the Deployment
If you have a new version of the Docker image (e.g., myapp:2.0), you can update the Deployment.
Edit the Deployment configuration:
kubectl set image deployment/myapp-deployment myapp-container=myapp:2.0
Verify the update:
kubectl rollout status deployment/myapp-deployment



