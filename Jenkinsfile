pipeline {
  agent any
  stages {
    stage('Build result') {
      steps {
        sh 'docker build -t tanguyn/ity:result ./result'
      }
    } 
    stage('Build vote') {
      steps {
        sh 'docker build -t tanguyn/ity:vote ./vote'
      }
    }
    stage('Build worker') {
      steps {
        sh 'docker build -t tanguyn/ity:worker ./worker'
      }
    }
    stage('Security scan') {
      steps {
        sh 'trivy fs . > git-security.log'
        sh 'trivy image docker.io/tanguyn/ity:vote > vote-security.log'
        sh 'trivy image docker.io/tanguyn/ity:result > result-security.log'
        sh 'trivy image docker.io/tanguyn/ity:worker > worker-security.log' 

      }
    }
    stage('Push result image') {
      steps {
        withDockerRegistry(credentialsId: 'dockerhubcredentials', url: '') {
          sh 'docker push tanguyn/ity:result'
        }
      }
    }
    stage('Push vote image') {
      steps {
        withDockerRegistry(credentialsId: 'dockerhubcredentials', url: '') {
          sh 'docker push tanguyn/ity:vote'
        }
      }
    }
    stage('Push worker image') {
      steps {
        withDockerRegistry(credentialsId: 'dockerhubcredentials', url: '') {
          sh 'docker push tanguyn/ity:worker'
        }
      }
    }
   
  }
  post {
        always {
            archiveArtifacts artifacts: 'git-security.log, vote-security.log, result-security.log, worker-security.log', onlyIfSuccessful: false
        }
    }
}

