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
                  sh "mvn clean verify sonar:sonar \
                     -Dsonar.projectKey=Sonarqube-test \
                     -Dsonar.host.url=http://ec2-54-157-253-76.compute-1.amazonaws.com:9000 \
                     -Dsonar.login=sqp_6be5bb54d24b53c752bb2e9a203333e77add7734"
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
