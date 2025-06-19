pipeline {
    agent any

    tools {
        maven 'Maven' // Ensure this matches your Jenkins tool config
    }

    stages {
        stage('Checkout from Git') {
            steps {
                git url: 'https://github.com/H19-AR/enahanced-petclinc-springboot.git', branch: 'prod'
            }
        }

        stage('Maven Compile') {
            steps {
                echo 'This is the Maven compile stage'
                sh 'mvn compile'
            }
        }

        stage('Maven Test') {
            steps {
                echo 'This is the Maven test stage'
                sh 'mvn test'
            }
        }
        stage('File System Scan By Trivy') {
            steps{
                echo 'Trivy Scanning Started'
                sh 'trivy fs --format table --output trivy-report.txt --sevirity HIGH,CRITICAL .'
            }
        }
    }
}
