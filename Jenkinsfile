pipeline {
    agent any
    tools {
        jdk 'Jdk11'
        maven 'maven-3.8.6'
    }
    stages {
        stage('Git Checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-secret', url: 'https://github.com/Angad-Raut/config-server.git']])
                bat 'mvn clean install'
                echo 'Git Checkout Completed'
            }
        }
        stage('Code Compile') {
            steps {
                bat 'mvn clean compile'
            }
        }
        stage('Unit Tests') {
            steps {
                bat 'mvn test'
            }
        }
        stage('Build Artifact') {
            steps {
                bat 'mvn clean package'
            }
        }
        stage('Archive Artifacts'){
            steps {
               archiveArtifacts artifacts: 'target/*.war'
            }
        }
        stage('Deploy on Tomcat') {
            steps {
                deploy adapters: [tomcat9(url: 'http://localhost:8086/',
                    credentialsId: 'tomcat-credentials')],
                    war: 'target/*.war',
                    contextPath: 'config-server'
            }
        }
    }
}
