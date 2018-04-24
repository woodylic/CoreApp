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
          sh "dotnet test CoreApp.Tests/CoreApp.Tests.csproj --logger:xunit --no-build --no-restore"
        }
      }
    }
    stage('Publish Test Results') {
      steps {
        script {
          step([$class: 'XUnitPublisher', 
            testTimeMargin: '3000', 
            thresholdMode: 1, 
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
          sh "dotnet test CoreApp.Tests/CoreApp.Tests.csproj --no-build --no-restore /p:CollectCoverage=true /p:CoverletOutputFormat=lcov"
        }
      }
    }
  }
}


dotnet ~/.nuget/packages/reportgenerator/4.0.0-alpha2/tools/ReportGenerator.dll 
  "-reports:/Users/andre/Projects/ReportGenerator/src/Testprojects/CSharp/Reports/OpenCover.xml" 
  "-targetdir:temp"