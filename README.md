# Instruction

This project is for testing unit test and code coverage.

## Build

```sh
dotnet restore
dotnet build CoreApp.sln -c Debug
```

## Unit Test

Add XunitXml.Logger

```sh
cd CoreApp.Tests
dotnet add package XunitXml.TestLogger --version 2.0.0
```

Execute test

```sh
dotnet test CoreApp.Tests/CoreApp.Tests.csproj --no-restore --no-build --logger:xunit
```

## Code Coverage

Add coverlet

```sh
cd CoreApp.Tests
dotnet add package coverlet.msbuild --version 1.1.1
```

Execute test

```sh
dotnet test CoreApp.Tests/CoreApp.Tests.csproj \
  --no-restore \
  --no-build \
  /p:CollectCoverage=true \
  /p:CoverletOutputFormat=opencover \
  /p:CoverletOutputDirectory=TestResults
```

## Generate Code Coverage Report

Add ReportGenerator

```sh
cd CoreApp.Tests
dotnet add package ReportGenerator --version 4.0.0-alpha3
```

Generate

```sh
dotnet ~/.nuget/packages/reportgenerator/4.0.0-alpha3/tools/ReportGenerator.dll \
  "-reports:**/TestResults/coverage.xml" \
  "-sourcedirs:CoreApp.Tests" \
  "-targetdir:artifacts/opencover"
```