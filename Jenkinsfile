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
                git 'https://github.com/mabayhan/sonarqubejava'
                sh 'mvn -Dmaven.test.failure.ignore=true clean package'
            }

            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'tests/target/*.jar'
                }
            }
        }

        stage('Static Code Analysis') {
            // docker:start
            // agent {
            //     docker {
            //         image 'sonarsource/sonar-scanner-cli'
            //     }
            // }
            // steps {
            //     withSonarQubeEnv('cicd') {
            //         echo '################# MAVEN ###################'
            //         sh 'sonar-scanner -Dsonar.projectKey=sonarqubejava:multimodule -Dsonar.sources=. -Dsonar.java.binaries=.'
            //     }
            // }
            // docker:end

            // maven:start
            agent {
                docker {
                    image "${MAVEN_IMAGE}"
                }
            }

            steps {
                withSonarQubeEnv('SonarQube Server') {
                    echo '################# MAVEN ###################'
                    sh 'mvn sonar:sonar'
                }
            }
        // maven:end
        }
    }
}
