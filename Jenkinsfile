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
        stage('Code Coverage') {
            steps {
                sh 'mvn -B jacoco:report'
                archiveArtifacts artifacts: 'target/site/jacoco/**', fingerprint: true
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'target/site/jacoco',
                    reportFiles: 'index.html',
                    reportName: 'JaCoCo Coverage Report'
                ])
                recordCoverage(tools: [[parser: 'JACOCO', pattern: 'target/site/jacoco/jacoco.xml']])
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
