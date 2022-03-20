import groovy.json.JsonSlurper
 
node {
    ws('netcore') {
        stage('SCM') {
            git branch: 'main', url: 'https://github.com/jlaguna27/aforo-git-pub'
        }
        stage('Build') {
            dotnet_build();
        }
      
        stage('Docker') {
            steps {
                
                sh(script: 'docker build -t jlaguna18/servicenet5 .', returnStdout: true);
                sh(script: 'docker push jlaguna18/servicenet5', returnStdout: true);
            }
        }
    
         stage('Deploy Dev') {
            steps {
                sh(script: 'az login --service-principal --username 89f99c6b-502f-44bc-8d12-dbe14c44b978 --password Raf.4~s_4-P99XvsDzinQ8pQxb1ZUirT.j --tenant 69155de0-5599-425f-8acd-bb00a6c86f0a', returnStdout: true);
                sh(script: 'az account set --subscription 2ea3df5b-495c-47f6-b919-e9564e0563fe "Developer"', returnStdout: true);
                sh(script: 'az container restart --name containerimage --resource-group prueba-Aforo', returnStdout: true);
                
            }
        }    
         stage('Deploy Prod') {
            steps {
                //bat(script: 'az aks get-credentials --resource-group aforo255Devops --name aforo255jenkins & kubectl config get-contexts --kubeconfig=%KUBE_PATH_CONFIG%', returnStdout: true);
                sh(script: 'az aks get-credentials --resource-group prueba-Aforo --name Kubernetes-Master & kubectl config get-contexts --kubeconfig=%KUBE-PATH-CONFIG%', returnStdout: true);
                sh(script: 'kubectl config use-context Kubernetes-Master --kubeconfig=%KUBE-PATH-CONFIG%', returnStdout: true);
                sh(script: 'Kubectl delete --all pods --kubeconfig=%KUBE-PATH-CONFIG% & kubectl apply -f k8s.yml --kubeconfig=%KUBE-PATH-CONFIG%', returnStdout: true);
            }
        }    
}
 
def dotnet_build() {
    sh(script: 'dotnet restore', returnStdout: true);
    sh(script: 'dotnet build', returnStdout: true);
    sh(script: 'dotnet test', returnStdout: true);
}
