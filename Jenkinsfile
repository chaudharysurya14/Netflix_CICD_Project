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
        // stage('Checkout from Git'){
        //     steps{
        //         echo '============================== GIT CHECKOUT =============================='
        //         git branch: 'master', url: 'https://github.com/chaudharysurya14/Netflix_CICD_Project.git'
        //     }
        // }
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
        stage('TRIVY FS SCAN') {
            steps {
                sh "trivy fs . > trivyfs.txt"
            }
        }
        stage("Docker Build & Push"){
            steps{
                echo '============================== DOCKER BUILD & PUSH =============================='
                script{
                   withDockerRegistry(credentialsId: 'docker_credentials', toolName: 'ocker_cred'){   
                       sh "docker build --build-arg TMDB_V3_API_KEY=3ad796c963c27c26487ac7d944e24532 -t netflix ."
                       sh "docker tag netflix surya0010/netflix:latest"
                       sh "docker push surya0010/netflix:latest"
                    }
                }
            }
        }
        stage("TRIVY"){
            steps{
                echo '============================== IMAGE SCANNING =============================='
                sh "trivy image surya0010/netflix:latest > trivyimage.txt" 
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
            to: 'postbox.suryahpcsa@gmail.com',
            attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
        }
    }
}
