# Deploying-Applications-Into-Kubernetes-Cluster


This project showcases the deployment of containerized applications as pods in Kubernetes and illustrates how to access the application through a web browser.

## STEP 1: Creating A Pod For The Nginx Application

To create an nginx pod, you can deploy it by applying the manifest file using the following command: `kubectl apply -f nginx-pod.yaml`.


**nginx-pod.yaml manifest file**

![k2](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/c8929267-51a9-422a-b08d-6aabcd369c81)

![k1](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/fec0e909-3dc0-4752-89ed-06e37d146bda)

- Running the following commands to inspect the setup:
```
$ kubectl get pod nginx-pod -o wide
```

![k3](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/d0bd52e4-af0f-4aec-b5fe-9cddd27f370d)

## STEP 2: Accessing The Nginx Application Through A Browser

To access the Nginx Pod within the Kubernetes cluster using its IP address, you need an image with curl software installed. Follow these steps:

- Execute the following **kubectl**  $ kubectl run curl --image=dareyregistry/curl -i --tty.
- Run the **curl** command, specifying the IP address of the Nginx Pod: $ curl -v 10.1.0.10:80.

![k4](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/6fa757a0-66af-4e1c-9a7d-32b9718d1521)

To enable access to the application through a browser, you need to create a service for the Nginx pod. 

**nginx-service.yaml manifest file**

![k6](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/9a6fc040-7d56-4d23-ae70-af103692ec4d)

Update the Pod manifest with the below and apply the manifest:

![k10](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/ec9ce3b6-bb97-4a04-914c-07ed4b8641b7)

Apply the manifest with:

`kubectl apply -f nginx-pod.yaml`

Create a nginx-service resource by applying your manifest

`kubectl apply -f nginx-service.yaml`

![k7](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/624c0a55-4ea7-408a-8728-8ef38db3a3b1)
 
Check the created service

`kubectl get service`

![k5](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/8a66b8ae-802e-41ce-8893-67c64f8351a2)

Now that we have a service created, how can we access the app? Since there is no public IP address, we can leverage kubectl's port-forward functionality.

![k8](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/d028bf66-00a9-45e1-9636-c26a2f3cb41e)

![k9](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/ecbaaf04-fe84-421a-b207-96abe3547732)

To make the Nginx app accessible through a browser using the NodePort service type, you can modify the nginx-service.yml manifest file. Simply add the NodePort as the type of service.

```
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx-pod
  ports:
    - protocol: TCP
      port: 80
      nodePort: 30080
```


![k12](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/c81b762d-d547-446f-99e9-9f165ba35ce6)

![k11](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/7787dfcf-4421-4784-aeef-cd2d4157101c)

## STEP 3: Creating A Replica Set

The purpose of a ReplicaSet object is to ensure the availability and stability of a set of pod replicas. It helps to maintain a consistent number of pod replicas running at all times, thereby enhancing availability in the event of pod failures or terminations.

- We Delete the nginx-pod:`kubectl delete pod nginx-pod` 

![k13](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/39f0eaa8-2528-4bd1-a89f-d8be0032ed8c)

-Create the replicaSet manifest file and apply using this command:`$ kubectl apply -f rs.yaml`

![k14](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/ddfb7db5-aa85-440a-9796-265b873c0de6)

Check the Pods and ReplicaSet Available by running command `kubectl get pods` and `kubectl get rs`

![k15](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/1eec67b9-f56c-477b-be19-2cb5de83f5f7)

![k16](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/8d051f1e-c62b-42ff-81dd-65615729835d)

If a pod is deleted within a ReplicaSet, the ReplicaSet will automatically initiate the scheduling of a replacement pod.

## STEP 4: Creating Deployment


A Deployment is a higher-level concept in Kubernetes that builds upon ReplicaSets and Pods. It introduces more advanced functionality compared to ReplicaSets. The primary role of a Deployment is to manage the deployment of ReplicaSets, offering convenient features such as seamless updates and the ability to rollback to previous versions. It operates in a declarative manner and facilitates rolling updates of microservices, guaranteeing uninterrupted service availability without downtime.

To utilize the service, follow these steps:

- Delete the previously created ReplicaSet using the command: `$ kubectl delete rs nginx-rs`.
- Create a deployment manifest file named *deployment.yaml*.
-Apply the deployment manifest file using the command:` $ kubectl apply -f deployment.yaml`.

![k19](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/4f01bc68-d58f-46ea-b5c8-f47b2180625e)

![k20](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/7d2ffa6c-17e6-4265-a632-a66b0d324d4e)

- Inspecting the setup:

```
$ kubectl get pods

$ kubectl get deploy

$ kubectl get rs
```

Exec into one of the Pod’s container to run Linux commands

![k21](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/308fa4e3-4f53-406a-8ff9-cbc057712ec7)

List the files and folders in the Nginx directory and Check the content of the default Nginx configuration file

![k22](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/ca3914db-3c9e-4a4a-b5ce-e4573691ecfb)




## STEP 5: DATA FOR PODS
Exec into the running container

To install Vim, you can use the following commands:

- Update the package lists:` apt-get update`.

- Install Vim: `apt-get install vim`.

![k24](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/15db2fed-1e06-4cc6-9b68-293fe238a758)

Update the content of the file and add the code below /usr/share/nginx/html/index.html

![k26](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/bb5f93c8-6d78-4df8-bac9-0dd6f7a5824f)


![k25](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/b111f546-3e92-4cca-8bb8-d57304592686)

Run the portforward command to access it on the websit `kubectl port-forward services/nginx.deployment 8080:80`

![k29](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/002e384d-31f1-49b6-a49c-2028fa603950)

Check the browser

![k27](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/3bca80be-af23-4387-8f88-ca56b2e15154)

Now, delete the only running Pod

![k31](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/0e9e4508-8d9e-45a5-8348-bd0219712a02)

Refresh the web page – You will see that the content you saved in the container is no longer there. That is because Pods do not store data when they are being recreated – that is why they are called ephemeral or stateless.

![k32](https://github.com/busolagbadero/Deploying-Applications-Into-Kubernetes-Cluster/assets/94229949/8583e16b-1d55-45ed-8fe2-bcaa8a633fcc)
















