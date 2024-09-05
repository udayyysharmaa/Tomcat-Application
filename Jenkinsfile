pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'jdk17'
    }
    environment{
        SONAR_HOME = tool "sonar-scanner"
    }

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/udayyysharmaa/Tomcat-Application.git'
            }
        }
        stage('Code Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Code Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Code Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Sonar Analysis') {
            steps {
                    withSonarQubeEnv('sonar') {
                        sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=Django-projectforclient -Dsonar.projectKey=Django-Projectforclient -Dsonar.java.binaries=target"

                }
            }
        }
        stage('Sonar Quality Check') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage('Code Deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'admin', path: '', url: 'http://13.127.110.176:8080')], contextPath: 'home', onFailure: false, war: 'target/*.war'
            }
        }
    }
}
