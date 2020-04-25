**Capstone project**


![capstone-pipeline](https://user-images.githubusercontent.com/19814105/80289721-c3bfb900-8740-11ea-82b8-30d69647e16f.PNG)

CICD pipeline does the following 
1. lint index.html -> linting mean that you make sure that syntax of html file is correct.(testing phase)
2. build docker image from docker file.
3. pull the image to aws ECR
4. deploy the image on aws eks(deploy phase)


**prerequisite**
1. installing jenkins and inside install aws, kubernetes and blue ocean plugins -> to create the pipeline
2. install aws cli, aws kubctl, aws iam authenticator -> to call cluster api to pods and host apps...


**the project in details**
1. create iam user for you to use it in "aws configure command" to call the cluster api.
2. from your pc terminal, create eks cluster from aws cli by the following command
aws cloudformation create-stack --stack-name cluster26042020 --template-body file://create_cluster.yaml --capabilities CAPABILITY_IAM
3. create node role by running the following command
aws cloudformation create-stack --stack-name node-role --template-body file://nodes-role.yaml --capabilities CAPABILITY_IAM
3. after it become active, create nodes from create node group and associtae nodes role with them
7. from your pc or open new instance, install the following
a. aws cli: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html 
b. aws iam authenticator: https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html
c.kubectl: https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
d. install docker
8. run this command to connect with your cluster "aws eks update-kubeconfig --name yourclustername"
9. build your docker image by this command "docker build -t capstonedocker ."  -> as it searches for the Dockerfile in the current directory
10. to list image now run "docker image ls"
11. pull your images to docker hub
12. then run this command  -> to push your docker to docker hub registery
kubectl create secret docker-registry registry-secret \
--docker-server=https:https://hub.docker.com/ \
--docker-username=your docker name \
--docker-password=your docker password \
--docker-email=your docker email
14. run this command "kubectl apply -f page1-controller.json", "kubectl apply -f pages2-controller.json" in page1, page2 folders
15. run this command "kubectl apply -f pages-service.json"  -> load balancer for your app 
16. run this command to see the services created  "kubectl get svc" and here you will see the endpoint and port so you can run your applicatin
17.make sure that you open the port which used in the blue-green.service.json either in your pc or ec2 instance
18.kubectl delete daemonsets,replicasets,services,deployments,pods,rc --all -> to delete all pods, deployments,... 
19- make sure that pod is in running status by running the commad "kubectl get pods" and in case pod not run try this command to see where the issue lies "kubectl decribe pods pod-name"
20-if everything is okay, now you can run "kubectl get svc" and you will find external ip and port.
21.if you open the pages-service.json and change selector from page1 to page 2 then rerun the command "kubectl apply -f pages2-controller.json" hint the websie again, you will find the website changed to the page2


there are 2 types of deployment (rolling or blue-green)
1- rolling means that you need to change index.html to contain addaitional part not added in the first index
2- blue-green -> to make 2 index.html but by changing the color (green and the other with blue)





creating CICD pipeline
1. install jenkins 
2. install blue ocean plugin 
3. install aws plugin 
4- install kubernetes plugin
5. add credentilas aws iam user in jenkins credentials to be able to upload docker image to ecr



**references**
1. to create cluster on aws
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html
https://logz.io/blog/amazon-eks-cluster/
2. to integrate kubernetes-cli-plugin with jenkins
 https://github.com/jenkinsci/kubernetes-cli-plugin/blob/master/README.md




