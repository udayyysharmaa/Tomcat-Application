pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven'
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
        stage('Code package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Sonar Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=TomcatApplication -Dsonar.projectKey=TomcatAplication -Dsonar.java.binaries=target "
                }
            }
        }
        stage('SonarGate Check') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage('Trivy Check') {
            steps {
                sh 'trivy fs --format table -o report.txt .'
            }
        }
        stage('Code Deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'admin', path: '', url: 'http://13.127.250.123:8080')], contextPath: 'home', onFailure: false, war: 'target/*.war'
            }
        }
    }
}
