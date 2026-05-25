pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3.9.9-eclipse-temurin-17'  // We use a Maven image with JDK 17
                    args '-v /home/jenkins/.m2:/root/.m2'  //Share local Maven repository
                }
            }
            steps {              
                        echo 'Compile project with maven'
                        sh 'mvn clean compile'                  
            }            
        }

        stage('Test') {
            agent {
                docker {
                    image 'maven:3.9.9-eclipse-temurin-17'  // We use the same Maven image for testing                    
                    args '-v /home/jenkins/.m2:/root/.m2'  // Share local Maven repository
                }
            }
            steps {               
                        echo 'Testing with maven'
                        sh 'mvn test'
                    }           
        }

        stage('Package') {
            agent {
                docker {
                    image 'maven:3.9.9-eclipse-temurin-17'  // We use the same Maven image for package
                    args '-v /home/jenkins/.m2:/root/.m2'  // Share local Maven repository
                }
            }
            steps {                  
                      echo 'Packing with maven'
                      sh 'mvn package'
                  }            
        }
        stage('Deploy') {
            input {
                    message 'Enter the miles to calculate'
                    parameters {
                            string 'miles'
                     }
                }
            agent {
                docker {
                    image 'maven:3.9.9-eclipse-temurin-17'  // We use the same Maven image for deploying
                    args '-v /home/jenkins/.m2:/root/.m2'  // Share local Maven repository
                }
                
            }
            steps {                
                        // Execute the jar file
                        echo 'Execute java application'
                        sh "java -cp 	target/miles-to-kilometers-1.0-SNAPSHOT.jar com.apasoft.App ${miles}"
                    }           
        }
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        success {
            echo 'Pipeline executed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
