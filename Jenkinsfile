pipeline {
    agent {
        node {
            label "java-nodo"
        }
    }

    stages { 
        stage("Build") {
            steps {
                sh "mvn clean package -DskipTest"
                
            }

        }
    }

}