pipeline {
    agent {
        node {
            label "nodo-java"
        }
    }

    stages {
         stage('Test') {
            steps {
                sh'Testing..'
            }
        }
        stage("Build") {
            steps {
                sh "mvn clean package -DskipTest"
                
            }

        }
    }

}
