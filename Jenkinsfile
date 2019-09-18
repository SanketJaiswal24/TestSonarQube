pipeline {
    agent any
    stages {
        
        stage('Compile Stage') {
             steps {
                 sh 'mvn clean compile'
             }      
        }

        stage('Test Stage') {
             steps {
                 sh 'mvn test'
             }      
        }
        
        stage('Clean Install Stage') {
             steps {
                 sh 'mvn clean install'
             }  
        }
        
        stage('SonarQube Check')
        {
          steps
          {
            withSonarQubeEnv ('Sonar')
            {
              sh 'mvn sonar:sonar   -Dsonar.host.url=http://localhost:9000   -Dsonar.login=b898832120c088619205a7fc9bd221c92b1cde86'
            }
          }
        }

       stage("Quality Gate"){
          timeout(time: 1, unit: 'MINUTES') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
                  slackSend baseUrl: 'https://hooks.slack.com/services/', channel: 'build', color: 'danger', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})", tokenCredentialId: 'slack-integration'
              }
          }
      }        
          
    }
    post {
        always {
    
        slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})") 
        
        }
        
        success {
            echo 'I am Success Done'
        }
        
        unstable {
            echo 'I am unstable :/'
        }
        
        failure {
          slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        
        changed {
            echo 'Things were different before...'
        }
        
    }
}
