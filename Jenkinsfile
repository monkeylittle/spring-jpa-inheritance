pipeline {
    agent any
    triggers {
        pollSCM('*/1 * * * *')
    }
    tools {
        jdk 'Java SE Development Kit 8'
        maven 'M3'
    }
    stages {
        stage('Code Commit') {
            steps {
                sh 'mvn package'
            }
            post {
                always {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn verify -Dskip.surefire.tests=true'
            }
            post {
                always {
                    junit 'target/surefire-reports/**/*.xml'
                }
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
        stage('Test') {
            failFast false
            parallel {
                stage('Acceptance Test') {
                    steps {
                        echo 'Acceptance Test Stage....'
                    }
                }
                stage('Performance Test') {
                    steps {
                        echo 'Performance Test Stage....'
                    }
                }
                stage('Stability Test') {
                    steps {
                        echo 'Stability Test Stage....'
                    }
                }
            }
        }
        stage('Exploratory Test') {
            steps {
                input message: "Promote to Exploratory Test?"
                echo 'Exploratory Test Stage....'
            }
        }
        stage('Production') {
            steps {
                input message: "Promote to Production?"
                echo 'Production Stage....'
            }
        }
    }
}
