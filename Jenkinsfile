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

   stage('Mutation Tests - PIT') {
      steps {
        sh "/opt/maven/bin/mvn org.pitest:pitest-maven:mutationCoverage"
      }
    }

stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t avisdocker/springhello-app-v1:""$GIT_COMMIT"" .'
          sh 'docker push avisdocker/springhello-app-v1:""$GIT_COMMIT""'
        }
      }
    }

 }
}
