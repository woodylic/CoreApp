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
          sh "dotnet restore && dotnet build CoreApp.sln -c Debug"
        }
      }
    }
    stage('Unit Test') {
      steps {
        script {
          sh '''
          dotnet test CoreApp.Tests/CoreApp.Tests.csproj \\
            --no-build \\
            --no-restore \\
            --logger:xunit
          '''
        }
      }
    }
    stage('Publish Test Results') {
      steps {
        script {
          step([$class: 'XUnitPublisher',
            tools: [
              [$class: 'XUnitDotNetTestType', 
                deleteOutputFiles: false, 
                failIfNotNew: true, 
                pattern: '**/TestResults/TestResults.xml', 
                skipNoTestFiles: false, 
                stopProcessingIfError: true
              ]
            ]
          ])
        }
      }
    }
    stage('Code Coverage') {
      steps {
        script {
          sh '''
          dotnet test CoreApp.Tests/CoreApp.Tests.csproj \\
            --no-build \\
            --no-restore \\
            /p:CollectCoverage=true \\
            /p:CoverletOutputFormat=cobertura \\
            /p:CoverletOutputDirectory=TestResults
          '''
        }
      }
    }
    stage('Generate Code Coverage Report') {
      steps {
        script {
          sh '''
          dotnet ~/.nuget/packages/reportgenerator/4.0.0-alpha3/tools/ReportGenerator.dll \
            "-reports:**/TestResults/coverage.xml" \
            "-sourcedirs:CoreApp.Tests" \
            "-targetdir:artifacts/opencover"
          '''
        }
      }
    }
  }
}
