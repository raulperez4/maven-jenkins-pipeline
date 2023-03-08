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
                     -Dsonar.projectKey=sonarqube \
                     -Dsonar.host.url=http://raul-jenkins-lb-689716188.us-                east-1.elb.amazonaws.com:9000 \
                     -Dsonar.login=sqp_47086855ace824bc986e8e468e6bd0085fa25606"
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
