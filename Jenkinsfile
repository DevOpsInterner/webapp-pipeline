pipeline {
  agent any
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
         sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
   
  stage ('GitCheckSecrets') {
      steps {
        sh 'rm trufflehog_results 2>/dev/null'
        sh 'docker run dxa4481/trufflehog --json https://github.com/DevOpsInterner/webapp-pipeline.git > trufflehog_results'
        sh 'cat trufflehog_results' 
       }
    }
    
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
   
    
     stage ('Deploy-To-Tomcat') {
            steps {
              sh 'scp -o StrictHostKeyChecking=no target/*.war  /opt/apache-tomcat-9.0.33/webapps/webapp-pipeline.war'
              }         
    }
    
    
  }

}
