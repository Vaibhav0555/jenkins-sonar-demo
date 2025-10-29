pipeline {
    agent any
    tools {
        jdk 'OpenJDK 21'   // This must match the JDK name you configured in Jenkins Global Tool Configuration
        maven 'Maven3'  // Similarly, your Maven tool name
    }
    stages{
        
        stage('checkout'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/vcjain/jenkins-sonar-demo.git']])
            }    
        }
        stage('Build'){
            steps{
                echo 'Building Maven project'
                sh 'mvn clean compile'
            }
        }
        stage('Test'){
            steps{
                echo 'Building Maven project'
                sh 'mvn test'
                junit '**/target/surefire-reports/*.xml'
                jacoco classPattern: '**/target/classes', exclusionPattern: '**/*Test*.class', execPattern: '**/target/jacoco.exec', inclusionPattern: '**/*.class', sourceExclusionPattern: 'generated/**/*.java', sourceInclusionPattern: '**/*.java'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                echo 'Scanning Maven project'
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv(installationName: 'sonarcloud', credentialsId: 'sonar-token') { 
                sh 'mvn sonar:sonar -Dsonar.projectKey=devops-by-vaibhav -Dsonar.organization=devops-by-vaibhav -Dsonar.host.url=https://sonarcloud.io' 
                    }
                }    
            }
        }
    }
        

}
