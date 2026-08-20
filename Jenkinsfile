pipeline {
    agent any
    tools {
    maven 'MavenLab'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -B test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Static Analysis') {
            steps {
                // report-only goal; does not fail the build on violations
                sh 'mvn -B spotbugs:spotbugs'
                archiveArtifacts artifacts: 'target/spotbugsXml.xml', fingerprint: true
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
