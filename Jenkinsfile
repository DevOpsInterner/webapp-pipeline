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
        sh 'rm trufflehog_results || true'
        sh 'docker run dxa4481/trufflehog --json https://github.com/DevOpsInterner/webapp-pipeline.git > trufflehog_results'
        sh 'cat trufflehog_results' 
       }
    }
    stage ('Dependencies Analysis') {
      steps {
          sh 'rm owasp* || true'
          sh '''
                  FILE=dependency-check.sh
                  if ! test -f "$FILE"; then
                     wget "https://raw.githubusercontent.com/DevOpsInterner/webapp-pipeline/master/dependency-check.sh"
                  fi
                  chmod +x dependency-check.sh
                  bash dependency-check.sh
             '''
       }
    }
  }
}
