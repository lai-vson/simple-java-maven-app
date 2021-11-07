pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
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
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
       stage('SONARqube') {
            steps {
                sh 'mvn sonar:sonar \
                    -Dsonar.projectKey=java \
                    -Dsonar.host.url=http://192.168.99.100:9000 \
                    -Dsonar.login=3fd49fb7989ed584e71e8a0bc869847a21db972b'
            }
       }
    }
}
