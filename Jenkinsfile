pipeline {
  agent { label 'dev' }

  stages {
    stage("Pulling from Github"){
      steps{
        git url:"https://github.com/mehdi621-git/tws-portfolio.git", branch : 'main'
      }
    }
    stage("Build") {
      steps {
        sh "docker build -t mehdi621docker/simpleportfolio ."
      }
    }

    stage("Push to Docker Hub") {
      steps {
        withCredentials([usernamePassword(credentialsId: 'cred', usernameVariable: 'userName', passwordVariable: 'userPass')]) {
          sh "docker login -u ${userName} -p ${userPass}"
          sh "docker push ${userName}/simpleportfolio"
        }
      }
    }

    stage("Pull from Docker Hub") {
      steps {
        sh "docker pull ${userName}/simpleportfolio"
      }
    }

    stage("Run the Container") {
      steps {
        sh "docker run -d -p 8080:8081 ${userName}/simpleportfolio"
      }
    }
  }
}
