pipeline {
  agent {
    docker { 
      label 'CN-PD-BACKEND-CI02'
      image 'microsoft/aspnetcore-build:2.0' 
      args '-u root:root' 
    }
  }
  stages {
    stage('Build') {
      steps {
        script {
          sh "dotnet restore && dotnet build CoreApp.sln -c Release"
        }
      }
    }
    stage('Unit Test') {
      steps {
        script {
          sh "dotnet test CoreApp.sln --logger:xunit"
        }
      }
    }
  }
}