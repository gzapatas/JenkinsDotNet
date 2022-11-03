pipeline {
    agent any
 
    stages {
        stage('SCM') {
            steps {
               git branch: 'main', url: 'https://github.com/gzapatas/JenkinsDotNet'
            }
        }
        
      stage ('Build Net6.0') {
           steps {
            sh(script: 'dir' , returnStdout:true);
            sh(script: 'dotnet restore' , returnStdout:true);
            sh(script: 'dotnet build' , returnStdout:true);
            sh(script: 'dotnet test' , returnStdout:true);
           }
       }
     stage ("Docker Build") {
           
           steps{
             // docker login
             sh(script: 'docker login --username ${UserDockerHub} --password ${PasswordDockerHub}', returnStdout:true)
             sh(script: 'docker build -t bgzapatas/servicenet6 .' , returnStdout:true);
             sh(script: 'docker push bgzapatas/servicenet6' , returnStdout:true);  
           }
       }   
       
       
        stage ("Deploy AKS") {
           steps {
            sh(script: 'az login --service-principal --username 81466b5b-e4ad-4553-adc7-3af1cf44b460 --password oGr8Q~lftkcwO-k~Dn~ZJBZuCb136yfB4.WjRcWq --tenant c4a66c34-2bb7-451f-8be1-b2c26a430158', returnStdout: true);
            sh(script: 'az account set --subscription Developer', returnStdout: true);
            sh(script: 'az aks get-credentials --resource-group MyCluster2_group --name MyCluster2', returnStdout: true);
            sh(script: 'kubectl config get-contexts --kubeconfig=${KUBE_PATH_CONFIG}', returnStdout: true);
            sh(script: 'kubectl config use-context MyCluster2 --kubeconfig=${KUBE_PATH_CONFIG}', returnStdout: true);
            sh(script: 'kubectl apply -f k8s.yml --kubeconfig=${KUBE_PATH_CONFIG}', returnStdout: true);
            sh(script: 'kubectl rollout restart deployment app-deployment --kubeconfig=${KUBE_PATH_CONFIG}', returnStdout: true);
           }
       }
    }
}