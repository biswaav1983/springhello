pipeline {
  agent any

  stages {

    stage('Build Artifact - Maven') {
      steps {
        sh "/opt/maven/bin/mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and JaCoCo') {
      steps {
        sh "/opt/maven/bin/mvn test"
      }
    }


stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t avisdocker/springhello-app-v1 .'
          sh 'docker push avisdocker/springhello-app-v1'
        }
      }
    }

 }
 post {
     always {
      junit 'target/surefire-reports/*.xml'
      jacoco execPattern: 'target/jacoco.exec'
    }
}

}
