node {
  checkout scm
  env.PATH = "${tool 'maven-3.6.3'}/bin:${env.PATH}"
  stage('Package') {
    dir('.') {
      sh 'mvn clean package -DskipTests'
    }
  }

  stage('Create Docker Image') {
    dir('.') {
      docker.build("movilidadagil/java_dockerized_with_jenkins:${env.BUILD_NUMBER}")
    }
  }

  stage ('Run Application') {
    try {
      // Start database container here
      // sh 'docker run -d --name db -p 8091-8093:8091-8093 -p 11210:11210 arungupta/oreilly-couchbase:latest'

      // Run application using Docker image

      // Run tests using Maven
      dir ('.') {
        sh 'mvn exec:java -DskipTests'
      }
    } catch (error) {
    } finally {
      // Stop and remove database container here
      //sh 'docker-compose stop db'
      //sh 'docker-compose rm db'
    }
  }

  stage('Run Tests') {
    try {
      dir('.') {
        sh "mvn test"
             docker.build("movilidadagil/java_dockerized_with_jenkins:${env.BUILD_NUMBER}").push()
      }
    } catch (error) {

    } finally {
      junit '**/target/surefire-reports/*.xml'
    }
  }
}
