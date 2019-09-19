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
            steps
            {
                script
                {
                 timeout(time: 60, unit: 'SECONDS') {
                  def qg = waitForQualityGate()
                   if (qg.status != 'OK') {
                  /* error "Pipeline aborted due to quality gate failure: ${qg.status}" */
                  }
                 }
                }
               } 
      }              
             
    }
    post {
        always {
        
        }
        
        success {
            echo 'I am Success Done'
        }
        
        unstable {
            echo 'I am unstable :/'
        }
        
        failure {
            echo 'I am failed :/'
        }
        
        changed {
            echo 'Things were different before...'
        }
        
    }
}
