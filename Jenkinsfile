pipeline {
   agent any  
   stages {
      stage('Git checkout') {
         steps {
         
            //clean workspace
               cleanWs()
            //checkout Bootcamp-Internal application into internal folder in Workspace
               checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'internal']], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/dnizam/bootcamp-internal.git']]])
            //checkout Bootcamp-External application into external folder in Workspace
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: 'external']], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/dnizam/bootcamp-external.git']]])
         }
      }
      stage('Build') {
         steps {
           sh '''
                // Get credentials for Cluster : 'user5-kube-cluster' and Project 'dtc-user5'
                  gcloud container clusters get-credentials user5-kube-cluster --zone us-central1-c --project dtc-user5
                
                //Run the Kubernetes deployments
                  kubectl apply -f internal/kube-config-internal.yaml
                  kubectl apply -f external/kube-config-external.yaml
                
                //Get Kubernetes Status 
                  kubectl get services
                  kubectl get deployments
                  kubectl get pods
            '''
         }
      }
   }
}
