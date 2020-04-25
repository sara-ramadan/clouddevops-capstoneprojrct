**Capstone project**


![capstone-pipeline](https://user-images.githubusercontent.com/19814105/80289721-c3bfb900-8740-11ea-82b8-30d69647e16f.PNG)

CICD pipeline does the following 
1. lint index.html -> linting mean that you make sure that syntax of html file is correct.(testing phase)
2. build docker image from dockerfile.
3. pull the image to aws ECR
4. deploy the image on aws eks(deployment phase)


**prerequisite**
1. installing jenkins and inside install aws, kubernetes and blue ocean plugins -> to create the pipeline
2. install aws cli, aws kubctl, aws iam authenticator -> to call cluster api to pods and host apps...
3. install docker


**the project in details**
1. create iam user for you to use it in "aws configure command" to call the cluster api.
2. from your pc terminal, create eks cluster from aws cli by the following command
aws cloudformation create-stack --stack-name cluster26042020 --template-body file://create_cluster.yaml --capabilities CAPABILITY_IAM
3. create node role by running the following command
aws cloudformation create-stack --stack-name node-role --template-body file://nodes-role.yaml --capabilities CAPABILITY_IAM
4. after the cluster become active, create nodes from create node group and associtae nodes role with them
![cluster on eks](https://user-images.githubusercontent.com/19814105/80292419-e957bd00-8756-11ea-89b1-02b6a9005df0.PNG)
5. run this command to connect with your cluster "aws eks update-kubeconfig --name yourclustername"
6. build your docker images by this command "docker build -t page1docker .", "docker build -t page1docker ."  -> in page1, page2 folders
7. to list image now run "docker image ls"
8. then run this command -> to push your docker images to docker hub registery
kubectl create secret docker-registry registry-secret \
--docker-server=https:https://hub.docker.com/ \
--docker-username=your docker name \
--docker-password=your docker password \
--docker-email=your docker email
9. run this command "kubectl apply -f page1-controller.json", "kubectl apply -f pages2-controller.json" in page1, page2 folders
10. run this command "kubectl apply -f pages-service.json"  -> load balancer for your app 
11- make sure that pod is in running status by running the commad "kubectl get pods" and in case pod not run try this command to see where the issue lies "kubectl decribe pods pod-name"
12-if everything is okay, now you can run "kubectl get svc" and you will find external ip and port. now you can hit this url on your browser and see now page1

![page2](https://user-images.githubusercontent.com/19814105/80293045-4013c580-875c-11ea-9800-577f1b341ca8.PNG)

13.if you open the pages-service.json and change selector from page1 to page 2 then rerun the command "kubectl apply -f pages-controller.json" then hint the websie again, you will find the website changed to the page2


![page1](https://user-images.githubusercontent.com/19814105/80293049-47d36a00-875c-11ea-9654-171e47856cc1.PNG)

14.kubectl delete daemonsets,replicasets,services,deployments,pods,rc --all -> in case you need to delete all pods, deployments,...


there are 2 types of deployment (rolling or blue-green)
1- rolling deplyment -> to add part by part to your app as you see in page1 , page2
2- blue-green -> to make the same app with different colors (green and blue) 

now we can move to creating the pipeline after makeing sure that every thing is working great.
---------------------------------------------------------------------------------------------

creating CICD pipeline
1. install jenkins 
2. install blue ocean plugin 
3. install aws plugin 
4- install kubernetes plugin
5. add credentilas aws iam user in jenkins credentials to be able to upload docker image to ecr

**the steps:**
click on blueocean plugin, connect with your github repo and it will read from your Jenkinsfile


**references**
1. to create cluster on aws
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html
https://logz.io/blog/amazon-eks-cluster/
2. to integrate kubernetes-cli-plugin with jenkins
 https://github.com/jenkinsci/kubernetes-cli-plugin/blob/master/README.md




