pipeline{
    agent any
    tools{
        jdk 'java11'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME=tool 'SonarQube-Scanner'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'master', url: 'https://github.com/chaudharysurya14/Netflix_CICD_Project.git'
            }
        }
        stage ('Software Composition Analysis') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'Owasp-DC'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        // stage("Sonarqube Analysis "){
        //     steps{
        //         withSonarQubeEnv('sonar-server') {
        //             sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Netflix \
        //             -Dsonar.projectKey=Netflix '''
        //         }
        //     }
        // }
        // stage("quality gate"){
        //    steps {
        //         script {
        //             waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' 
        //         }
        //     } 
        // }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
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
    //         to: 'postbox.aj99@gmail.com',
    //         attachmentsPattern: 'trivyfs.txt,trivyimage.txt'
    //     }
    // }
}
