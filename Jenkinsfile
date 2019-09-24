pipeline {
    agent {
        docker {
            image 'tadaszi/maven' 
            args '-v /C/Users/tadaszi/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        
        stage('Test') { 
            steps {
                sh 'mvn test' 
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' 
                }
            }
        }
        stage('Test2') { 
            parallel {
               stage("Test2.2") {
                   steps {
                        sh 'mvn test' 
        				input message: 'Finished testing? (Click "Proceed" to continue)'
                   }
               }
               stage("Echo1") {
                   steps {
                       echo "echo111";
                   }
               }
               stage("Echo2") {
                   steps {
                       echo "echo222";
                   }
               }
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' 
                }
            }
        }
		stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}