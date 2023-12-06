pipeline{
    agent any
    tools{
        jdk 'java11'
        nodejs 'node16'
    }
    environment {
    // defining sonarqube enviroment
    SONAR_SCANNER = 'SonarQube-Scanner'
    // defining sonarqube server
    SONAR_SERVER = 'sonarqube'
    }
    stages {
        stage('clean workspace'){
            steps{
                echo '============================== CLEAN WORKSHOP =============================='
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                echo '============================== GIT CHECKOUT =============================='
                git branch: 'master', url: 'https://github.com/chaudharysurya14/Netflix_CICD_Project.git'
            }
        }
        stage ('Software Composition Analysis') {
            steps {
                echo '============================== DEPENDENCY CHECK =============================='
                dependencyCheck additionalArguments: ''' 
                    -o "./" 
                    -s "./"
                    -f "ALL" 
                    --prettyPrint''', odcInstallation: 'Owasp-DC'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
        stage('Install Dependencies') {
            steps {
                echo '============================== INSTALL DEPENDENCY =============================='
                sh "npm install"
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                echo '============================== STATIC ANALYSIS =============================='
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn clean sonar:sonar -Dsonar.javabinaries=src -Dsonar.projectName=Netflix' 
                    // sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Portfolio_CICD_Project \
                    // -Dsonar.projectName=Portfolio_CICD_Project \ 
                    // -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
                }
            }
        }
        // stage("quality gate"){
        //    steps {
        //         script {
        //             waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' 
        //         }
        //     } 
        // }
        
    }
    // post {
    //  always {
    //     emailext attachLog: true,
    //         subject: "'${currentBuild.result}'",
    //         body: "Project: ${env.JOB_NAME}<br/>" +
    //             "Build Number: ${env.BUILD_NUMBER}<br/>" +
    //             "URL: ${env.BUILD_URL}<br/>",
    //         to: 'postbox.aj99@gmail.com',
    //         attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
    //     }
    // }
}
