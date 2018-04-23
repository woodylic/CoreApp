pipeline {
  agent {
    docker { 
      image 'microsoft/aspnetcore-build:2.0' 
      args '-u root:root' 
    }
  }
  stages {
    stage('Build') {
      steps {
        script {
          sh "dotnet build CoreApp.sln -c Release"
        }
      }
    }
  }
}