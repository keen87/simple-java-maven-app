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
        
        stage('shell') {
            steps {
                sh 'echo $pwd'
                sh 'ls'
                sh 'echo abc > abc'
                sh 'ls'
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
               stage("Echo1") {
                   steps {
                       sh "echo $pwd";
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