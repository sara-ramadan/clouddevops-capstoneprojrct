pipeline {
    agent any
    environment {
        registry = '768362009725.dkr.ecr.us-east-1.amazonaws.com/capstone_udacity_project:latest'
    }
    stages {
    
        stage("lint HTML") {
           steps {
                 sh 'tidy -q -e *.html'
            }
        }
    
        stage("Docker Build") {
            steps {
                sh "docker build -t capstone_udacity_project ."
                sh "docker tag capstone_udacity_project:latest ${registry}"
            }
        }
        stage("ECR Login") {
            steps {
                withAWS(credentials:'aws_credential') {
                    script {
                        def login = ecrLogin()
                        sh "${login}"
                    }
                }
            }
        }
        stage("Docker Push") {
            steps {
                sh "docker push ${registry}"
            }
        }
    }
    stage('deploy on aws eks') {
        steps {
            withKubeConfig([credentialsId: 'id', serverUrl: 'https://AC8117AFE12A001CAB62D0B6C5AE7B25.yl4.us-east-1.eks.amazonaws.com']) {
                sh 'kubectl apply -f /home/ubuntu/clouddevops-capstoneprojrct/page1/page1-controller.json'
                sh 'kubectl apply -f /home/ubuntu/clouddevops-capstoneprojrct/page2/page2-controller.json'
                sh 'kubectl apply -f /home/ubuntu/clouddevops-capstoneprojrct/pages-service.json'
                sh 'kubectl get svc'
            }
            
        }
  }
    
    
    
}
