node {
  checkout scm
  env.PATH = "${tool 'maven-3.5.2'}/bin:${env.PATH}"
  stage('Package') {
    dir('JavaDockerizedWithJenkins') {
      sh 'mvn clean package -DskipTests'
    }
  }

  stage('Create Docker Image') {
    dir('JavaDockerizedWithJenkins') {
      docker.build("movilidadagil/JavaDockerizedWithJenkins:${env.BUILD_NUMBER}")
    }
  }

  stage ('Run Application') {
    try {
      // Start database container here
      // sh 'docker run -d --name db -p 8091-8093:8091-8093 -p 11210:11210 arungupta/oreilly-couchbase:latest'

      // Run application using Docker image

      // Run tests using Maven
      dir ('JavaDockerizedWithJenkins') {
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
      dir('webapp') {
        sh "mvn test"
        docker.build("arungupta/docker-jenkins-pipeline:${env.BUILD_NUMBER}").push()
      }
    } catch (error) {

    } finally {
      junit '**/target/surefire-reports/*.xml'
    }
  }
}
