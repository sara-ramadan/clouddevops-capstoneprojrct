#follow this links to create cluster and attach them with nodes.
https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html
https://logz.io/blog/amazon-eks-cluster/

1. create iam user for you to use it in the next steps
2. creating stack for vpc and subnets 
3. creating stack for nodes role
4. create iam role for eks cluster which gives it the permission to manage ec2 nodes
5. create cluster using the above vpc, subnets , iam role from Elastic Kubernetes Service
6. after it become active, create nodes from create node group and associtae nodes role with them
7. from your pc or open new instance and download the following
a. aws cli: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html 
b. aws iam authenticator: https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html
c.kubectl: https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
d. install docker
8. then run "aws configure" and use the iam user you created the 1st time.
9. run this command to connect with your cluster "aws eks update-kubeconfig --name yourclustername"
10. build your docker image by this command "docker build -t capstonedocker ."  -> as it searches for the Dockerfile in the current directory
11. to list image now run "docker image ls"
12. pull your images to docker hub
13. then run this command 
kubectl create secret docker-registry registry-secret \
--docker-server=https:https://hub.docker.com/ \
--docker-username=your docker name \
--docker-password=your docker password \
--docker-email=your docker email
14. run this command "kubectl apply -f page1-controller.json", "kubectl apply -f pages2-controller.json" in page1, page2 folders
15. run this command "kubectl apply -f pages-service.json"  
16. run this command to see the services created  "kubectl get svc" and here you will see the endpoint and port so you can run your applicatin
17.make sure that you open the port which used in the blue-green.service.json either in your pc or ec2 instance
18.kubectl delete daemonsets,replicasets,services,deployments,pods,rc --all -> to delete all pods, deployments,... 
19- make sure that pod is in running status by running the commad "kubectl get pods" and in case pod not run try this command to see where the issue lies "kubectl decribe pods pod-name"
20-if everything is okay, now you can run "kubectl get svc" and you will find external ip and port.
21.if you open the pages-service.json and change selector from page1 to page 2 then rerun the command "kubectl apply -f pages2-controller.json" hint the websie again, you will find the website changed to the page2


there are 2 types of deployment (rolling or blue-green)
1- rolling means that you need to change index.html to contain addaitional part not added in the first index
2- blue-green -> to make 2 index.html but by changing the color (green and the other with blue)


