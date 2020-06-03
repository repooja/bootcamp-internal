pipeline{
    agent any
    environment {
        Image_name = "gcr.io/dtc-user7/internal-image:V_${BUILD_ID}"
    }
    stages{
        stage('dependancy versions'){
            steps{
                sh '''
                    git --version
                    docker --version
                    npm -v
                '''
            }    
        }
        stage('git checkout'){
            steps{
                git 'https://github.com/dnizam/bootcamp-internal.git'
            }    
        }
        stage('git test'){
            steps{
                sh '''
                    ls -a
                    echo "install dependencies and test internal code ..!"
                    npm install
                    npm test
                ''' 
            }    
        }
        stage('build'){
            steps{
                sh '''
                    echo "${Image_name}"
                    echo "build and push docker image for internal app ..!"
                    gcloud builds submit --tag ${Image_name} .
                ''' 
            }    
        }
        stage('deploy'){
            steps{
                sh '''
                    gcloud container clusters get-credentials cluster-1 --zone us-central1-c --project dtc-user7
                    kubectl set image deployment/events-data events-data=${Image_name} --namespace=development
                ''' 
            }    
        }  
    }
}