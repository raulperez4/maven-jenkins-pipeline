pipeline {
  agent any
  tools {
        maven "Maven 3.9.0" 
  }

  stages {

      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }  
      }

      stage('Test Maven - JUnit') {
            steps {
              sh "mvn test"
            }
            post{
              always{
                junit 'target/surefire-reports/*.xml'
              }
            }
      }
      
      stage('Sonarqube Analysis - SAST') {
            steps {
                  withSonarQubeEnv('SonarQube') {
                  sh "mvn sonar:sonar \
                  -Dsonar.projectKey=maven-jenkins-pipeline \
                  -Dsonar.host.url=http://jenkins.orexe.biz:9000"
                  }
              timeout(time: 2, unit: 'MINUTES') {
                script {
                waitForQualityGate abortPipeline: true
                }
              }
            }
      }
  }
}
