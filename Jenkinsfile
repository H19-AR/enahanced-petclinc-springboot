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
            steps {
                echo 'Trivy Scanning Started'
                sh 'trivy fs --format table --output trivy-report.txt --severity HIGH,CRITICAL .'
            }
        }

        stage('Sonar Analysis') {
            environment {
                SCANNER_HOME = tool 'Sonar-scanner'  // Make sure 'Sonar-scanner' is defined in Jenkins Global Tool Configuration
            }
            steps {
                withSonarQubeEnv('sonarserver') {  // Make sure 'sonarserver' is defined in Jenkins Configuration
                    sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.organization=harshachintala \
                        -Dsonar.projectName=SpringBootPet \
                        -Dsonar.projectKey=harshachintala_springbootpet \
                        -Dsonar.java.binaries=. \
                        -Dsonar.exclusions=**/trivy-report.txt
                    '''
                }
            }
        }

        // Sonar Quality Gate stage
        stage('Sonar Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true, credentialsId: 'sonar'
                }
            }
        }
    }
}
