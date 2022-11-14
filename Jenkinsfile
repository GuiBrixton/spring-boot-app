pipeline {
    agent {
        node {
            label "nodo-java"
        }
    }

    stages { 
         stage("Test"){
            steps {
                junit "/target/test-results.xml"
            }
         }
        stage("Build") {
            steps {
                sh "mvn clean package -DskipTest"
                
            }

        }
    }

}
