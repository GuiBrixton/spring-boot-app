pipeline{

    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: acavaleiro/jenkins-nodo-java-bootcamp:1.0 
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-socket-volume
    securityContext:
      privileged: true
  volumes:
  - name: docker-socket-volume
    hostPath:
      path: /var/run/docker.sock
      type: Socket
    command:
    - sleep
    args:
    - infinity
        '''
            defaultContainer 'shell'
        }
    }

  environment {
    registryCredential='acavaleiro'
    registryFrontend = 'acavaleiro/spring-boot-app'
  }

  stages {
    stage("Builds"){
        steps{
            sh "mvn clean package -DskipTests"
        }
    }

    stage('Push Image to Docker Hub') {
      steps {
        script {
          dockerImage = docker.build registryFrontend + ":$BUILD_NUMBER"
          docker.withRegistry( '', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Push Image latest to Docker Hub') {
      steps {
        script {
          dockerImage = docker.build registryFrontend + ":latest"
          docker.withRegistry( '', registryCredential) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy to K8s') {
      steps{
        script {
          if(fileExists("configuracion")){
            sh 'rm -r configuracion'
          }
        }
        sh "git clone https://github.com/Guibrixton/kubernetes-helm-docker-config.git configuracion --branch test-implementation"
        sh "kubectl apply -f configuracion/kubernetes-deployment/spring-boot-app/manifest.yaml --kubeconfig=configuracion/kubernetes-config/config"
      }
    }
  }
  post {
    always {
      sh 'docker logout'
    }
  }
}

