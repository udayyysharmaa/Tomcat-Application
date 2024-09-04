pipeline {
    agent any
    tools {
        jdk 'jdk17'
        maven 'maven'
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
        stage('Code Deploy') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'admin', path: '', url: 'http://3.108.234.204:8080/')], contextPath: 'home', onFailure: false, war: 'target/*.war'
            }
        }
    }
}
