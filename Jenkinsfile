pipeline {
  agent any
  options {
    buildDiscarder(logRotator(numToKeepStr: '5'))
  }
  environment {
    
    DOCKERHUB_CREDENTIALS = credentials('dockerhub')
  }
  stages {



    
    
    
          stage('Docker Bench Security') {
      steps {
        sh 'chmod +x docker-bench-security.sh'
        sh './docker-bench-security.sh'
      }
    }


     stage('SonarQube Analysis') {
          agent any
      steps {
       

       sh '/var/opt/sonar-scanner-4.7.0.2747-linux/bin/sonar-scanner -Dsonar.projectKey=cartservice -Dsonar.sources=. -Dsonar.host.url=http://172.31.7.193:9000 -Dsonar.token=sqp_c0619a6d815dcefbf5c27b67c3d688e9f4cccae3'

        
      }
    }





    stage('Build Docker Image') {
            steps {
                script {
                    def dockerImage = docker.build('koushiksai/cartservice:latest', '.')
                }
            }
        }


    stage('Login') {
      steps {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    stage('Push') {
      steps {
        sh 'docker push koushiksai/cartservice'
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}