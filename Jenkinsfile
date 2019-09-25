pipeline {
    agent {
        docker {
            image 'tadaszi/maven' 
            args '-v /C/Users/tadaszi/.m2:/root/.m2' 
        }
    }
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }
    environment {
        CUSTOM_VAR = "CUSTOM_VAR_value" 
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        
        stage('shell') {
            steps {
                sh 'cat /etc/os-release'
                sh 'printenv'
//                input {
//                    message "Should we continue?"
//                    ok "Yes, we should."
//                    parameters {
//                        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
//                    }
//                }
                sh 'printenv'
            }
        } 
        
        stage('Test') { 
            steps {
                sh 'ls'
                sh 'mvn test'
                sh 'ls' 
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' 
                }
            }
        }
        
        stage('ChoiceOne') {
            when {
                environment name: 'CHOICE', value: 'ONE'
            }
            steps {
                sh 'ls'
            }
        }
        
        stage('Test2') { 
            parallel {
               stage("Echo1") {
                   steps {
                       sh "echo choiceVal=${CHOICE}"
                       sh "echo choiceVal=${choice}"
                   }
               }
               stage("Echo2") {
                   steps {
                       sh "echo echo2 > echo2";
                       sh "echo echo222";
                       sh "ls";
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
                sh "ls"
            }
        }
    }
}