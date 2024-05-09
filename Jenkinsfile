pipeline {
    agent any
    /*tools{
        jdk 'jdk17'
        nodejs 'node16'
    }*/
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage('clean workspace') {
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git') {
            steps{
                git branch: 'main', url: 'https://github.com/typedot/Myntra-Clone-App.git'
            }
        }
        /*stage("Sonarqube Analysis") {
            steps{
                withSonarQubeEnv(installationName: 'sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=myntra-clone-app-3 -Dsonar.projectKey=myntra-clone-app-3 '''
                }                
            }            
        }*/
                /*withSonarQubeEnv('sonar-server') {
                    sonar-scanner \
                      -Dsonar.projectKey=myntra-clone-app-2 \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=http://34.0.8.183:9000 \
                      -Dsonar.token=sqp_2d8093adeae82e4a14296829dd10d3f9740f99e7
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    timeout(time: 2, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true, credentialsId: 'sonarqube'
                    }
                }
            } 
        }*/
        /*stage("status check") {
            steps {
                echo "Status: SonarQube Quality Gate Success!!"
            }
        }*/
        /*stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }*/
        stage("Docker Build & Push") {
            steps{
                script{
                   withDockerRegistry(credentialsId: 'dockerhub', toolName: 'dockertool'){   
                       sh "docker build -f Dockerfile -t myntra-clone-app-01 ."
                       sh "docker tag myntra-clone-app-01 rover102/myntra-clone-app-01:${BUILD_NUMBER} "
                       sh "docker push rover102/myntra-clone-app-01:${BUILD_NUMBER} "
                    }
                }
            }
        }
        stage('Update Deployment File') {
            environment {
                GIT_REPO_NAME = "myntra-clone-app-manifests"
                GIT_USER_NAME = "typedot"
            }
            steps {
                withCredentials([string(credentialsId: 'github', variable: 'GITHUB_TOKEN')]) {
                    sh '''
                        git config user.email "delhi.keshav@gmail.com"
                        git config user.name "typedot"
                        ReplaceImageTag=${BUILD_NUMBER}
                        git clone https://github.com/typedot/myntra-clone-app-manifests.git
                        pwd
                        cd myntra-clone-app-manifests/
                        pwd
                        ls -ll
                        cd myntra-manifest/
                        pwd
                        ls -ll
                        sed -i "s/ReplaceImageTag/${BUILD_NUMBER}/g" deployment-myntra-clone-app.yaml
                        git add .
                        git commit -m "Updated deployment image to version ${BUILD_NUMBER}"
                        git push https://${GITHUB_TOKEN}@github.com/${GIT_USER_NAME}/${GIT_REPO_NAME} HEAD:main
                    '''
                }
            }
        }
        /*stage("TRIVY"){
            steps{
                sh "trivy image nasi101/netflix:latest > trivyimage.txt" 
            }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d -p 8081:80 nasi101/netflix:latest'
            }
        }
        stage('Deploy to kubernets'){
            steps{
                script{
                    dir('Kubernetes') {
                        withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                                sh 'kubectl apply -f deployment.yml'
                                sh 'kubectl apply -f service.yml'
                        }   
                    }
                }
            }
        }

    }
    post {
     always {
        emailext attachLog: true,
            subject: "'${currentBuild.result}'",
            body: "Project: ${env.JOB_NAME}<br/>" +
                "Build Number: ${env.BUILD_NUMBER}<br/>" +
                "URL: ${env.BUILD_URL}<br/>",
            to: 'iambatmanthegoat@gmail.com',                                #change mail here
            attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
        }
    }*/
    }
}
