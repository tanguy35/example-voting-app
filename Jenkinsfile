pipeline {
  agent any
  stages {
    stage('Build result') {
      steps {
        sh 'docker build -t spywash/devops:result ./result'
      }
    } 
    stage('Build vote') {
      steps {
        sh 'docker build -t spywash/devops:vote ./vote'
      }
    }
    stage('Build worker') {
      steps {
        sh 'docker build -t spywash/devops:worker ./worker'
      }
    }
    stage('Security scan') {
      steps {
        sh 'trivy fs . > git-security.log'
        sh 'trivy image docker.io/spywash/devops:vote > vote-security.log'
        sh 'trivy image docker.io/spywash/devops:result > result-security.log'
        sh 'trivy image docker.io/spywash/devops:worker > worker-security.log' 
      }
    }
    stage('Push result image') {
      steps {
        withDockerRegistry(credentialsId: 'dockerhubcredentials', url: '') {
          sh 'docker push spywash/devops:result'
        }
      }
    }
    stage('Push vote image') {
      steps {
        withDockerRegistry(credentialsId: 'dockerhubcredentials', url: '') {
          sh 'docker push spywash/devops:vote'
        }
      }
    }
    stage('Push worker image') {
      steps {
        withDockerRegistry(credentialsId: 'dockerhubcredentials', url: '') {
          sh 'docker push spywash/devops:worker'
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
