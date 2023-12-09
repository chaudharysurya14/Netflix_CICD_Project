pipeline{
    agent any
    tools{
        jdk 'java11'
        nodejs 'node16'
    }
    environment {
    // defining sonarqube enviroment
    SCANNER_HOME=tool 'SonarQube-Scanner'
    // defining sonarqube server
    SONAR_SERVER = 'sonarqube'
    }
    stages {
        // stage('clean workspace'){
        //     steps{
        //         echo '============================== CLEAN WORKSHOP =============================='
        //         cleanWs()
        //     }
        // }
        stage('Checkout from Git'){
            steps{
                echo '============================== GIT CHECKOUT =============================='
                git branch: 'master', url: 'https://github.com/chaudharysurya14/Netflix_CICD_Project.git'
            }
        }
        // stage ('Software Composition Analysis') {
        //     steps {
        //         echo '============================== DEPENDENCY CHECK =============================='
        //         dependencyCheck additionalArguments: ''' 
        //             -o "./" 
        //             -s "./"
        //             -f "ALL" 
        //             --prettyPrint''', odcInstallation: 'Owasp-DC'
        //         dependencyCheckPublisher pattern: 'dependency-check-report.xml'
        //     }
        // }
        // stage('Install Dependencies') {
        //     steps {
        //         echo '============================== INSTALL DEPENDENCY =============================='
        //         sh "npm install"
        //     }
        // }
        // stage("Sonarqube Analysis "){
        //     steps{
        //         echo '============================== STATIC ANALYSIS =============================='
        //         withSonarQubeEnv('sonarqube') {
        //             sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
        //             -Dsonar.projectKey=Netflix '''
        //             // sh 'mvn clean sonar:sonar -Dsonar.javabinaries=src -Dsonar.projectName=Netflix' 
        //             // sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Portfolio_CICD_Project \
        //             // -Dsonar.projectName=Portfolio_CICD_Project \ 
        //             // -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
        //         }
        //     }
        // }
        // stage("quality gate"){
        //    steps {
        //         echo '============================== CHECK SONARQUBE QUALITY =============================='
        //         script {
        //             waitForQualityGate abortPipeline: false, credentialsId: 'sonarkey' 
        //         }
        //     } 
        // }
        // stage('TRIVY FILES SCAN') {
        //     steps {
        //         echo '============================== TRIVY FILE SCANNING =============================='
        //         sh "trivy fs . > trivyfs.txt"
        //     }
        // }
        // stage("Docker Build"){
        //     steps{
        //         echo '============================== DOCKER BUILD =============================='
        //         script{
        //             withDockerRegistry(credentialsId: 'docker_credentials', toolName: 'ocker_cred'){   
        //                 sh "docker build --build-arg TMDB_V3_API_KEY=3ad796c963c27c26487ac7d944e24532 -t netflix ."
        //             }
        //         }
        //     }
        // }
        // stage("Docker push"){
        //     steps{
        //         echo '============================== DOCKER PUSH =============================='
        //         script{
        //            withDockerRegistry(credentialsId: 'docker_credentials', toolName: 'ocker_cred'){   
        //                sh "docker tag netflix surya0010/netflix:$BUILD_ID"
        //                sh "docker push surya0010/netflix:$BUILD_ID"
        //             }
        //         }
        //     }
        // }
        // stage("TRIVY IMAGE SCAN"){
        //     steps{
        //         echo '============================== TRIVY IMAGE SCANNING =============================='
        //         sh "trivy image surya0010/netflix:latest > trivyimage.txt" 
        //     }
        // }
        stage('Deploy to container'){
            steps{
                echo '============================== DEPLOY ON DOCKER =============================='
                sh "docker stop netflix" || true
                sh "docker rm netflix" || true
                sh "docker run -d --name netflix -p 8081:80 surya0010/netflix:36"
            }
        }
        stage('Deploy to kubernets'){
            steps{
                echo '============================== DEPLOY ON K8s =============================='
                script{
                    dir('Kubernetes') {
                        withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'K8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                            sh "kubectl apply -f deployment.yml"
                            sh "kubectl apply -f service.yml"
                        }    
                    }
                }
            }
        }  
    }
    // post {
    //  always {
    //     emailext attachLog: true,
    //         subject: "'${currentBuild.result}'",
    //         body: "Project: ${env.JOB_NAME}<br/>" +
    //             "Build Number: ${env.BUILD_NUMBER}<br/>" +
    //             "URL: ${env.BUILD_URL}<br/>",
    //         to: 'postbox.suryahpcsa@gmail.com',
    //         attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
    //     }
    // }
}
