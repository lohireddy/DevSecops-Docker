pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "maven_new"
    }

    stages {
        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests=true"
                archive 'target/*.jar'
            }
        }
           stage('Test') {
            steps {
                sh "ls ; mvn test"
            }
            post{
                always {
                    junit 'target/surefire-reports/*.xml'
                    jacoco (execPattern: 'target/jacoco.exec')
                  }
             }
        }
           stage('Docker Build and Push') {
            steps {
              withDockerRegistry(credentialsId: 'docker-hub', url: 'https://github.com/lohireddy/DevSecops-Docker.git') {
                sh 'printenv'
                sh 'docker build -t docker.io/lohireddy/numeric-app:""$GIT_COMMIT"" .'
                sh 'docker push docker.io/lohireddy/numeric-app:""$GIT_COMMIT""'
            }
         }
      }
   }   
}
