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
          sh 'docker build -t avisdocker/springhello .'
          sh 'docker push avisdocker/springhello'
        }
      }
    }


stage('Docker Run') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker -d -p 5552:5552  avisdocker/springhello'
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
