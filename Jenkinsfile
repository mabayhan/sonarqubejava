def MAVEN_IMAGE = 'maven:3.8.2-openjdk-11-slim'

pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image "${MAVEN_IMAGE}"
                }
            }
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/mabayhan/sonarqubejava'

                // Run Maven on a Unix agent.
                sh 'mvn -Dmaven.test.failure.ignore=true clean package'

            // To run Maven on a Windows agent, use
            // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'tests/target/*.jar'
                }
            }
        }

        stage('Static Code Analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli'
                }
            }
            steps {
                withSonarQubeEnv('cicd') {
                    sh 'sonar-scanner -Dsonar.projectKey=sonarqubejava:multimodule -Dsonar.sources=. -Dsonar.java.binaries=.'
                }
            }
        }
    }
}
