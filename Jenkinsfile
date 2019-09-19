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
              sh 'mvn sonar:sonar -Dsonar.projectKey=devops-demo -Dsonar.host.url=http://localhost:9000 -Dsonar.login=2c6d7ae3a260791ea85d63dca84e1fb8dd2310cd'
            }
          }
        }
        
         stage("Quality Gate"){
              
              parallel("first": {
                   timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
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
