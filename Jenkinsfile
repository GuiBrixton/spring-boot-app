pipeline {
    agent {
        node {
            label "nodo-java"
        }
    }

    stages {
         stage('Test') {
            steps {
                jacoco()
                junit "target/surefire-reports/*.xml"
                
            }
        }
        stage("Build") {
            steps {
                sh "mvn clean package -DskipTest"
                
            }

        }
    }

}
