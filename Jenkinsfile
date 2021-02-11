pipeline {
  envinorment{
    DOCKER_MAVEN_IMAGE = 'maven:3.5.2-jdk-8-alpine'
    DOCKER_MAVEN_ARGS = '-v $HOME/.m2/builds/$BRANCH_NAME:/root/.m2 -u 0:0'
  }
  stages{
    stage('load') {
      agent {
        docker {
          image DOCKER_MAVEN_IMAGE
          args DOCKER_MAVEN_ARGS
        }
      }
    }
      stage('Test') {
        steps {
            
            // cleanup generate artifacts to ensure build can be cleaned up
            sh 'mvn -version'
        }
      }
  }
}
