node{
    cleanWs()
    stage ('Checkout Scm'){
        git 'https://github.com/gopinath45/Sample-Dotnet-Webapp-SP.git'
    }
    
    stage ('Restore Nuget Packages'){
        bat 'C:\\Tools\\Nuget\\nuget.exe restore "%WORKSPACE%\\WebApplication1\\WebApplication1.sln"'
    }
    
    stage('Build + SonarQube analysis') {
        def sqScannerMsBuildHome = tool 'Scanner_MSBuild_5.3.1'
        withSonarQubeEnv('Sonarqube') {
          bat "${sqScannerMsBuildHome}\\SonarScanner.MSBuild.exe begin /k:dotnet"
          bat "MSBuild.exe WebApplication1\\WebApplication1.sln /t:Rebuild"
          bat "${sqScannerMsBuildHome}\\SonarScanner.MSBuild.exe end"
        }
    }
    
    stage("Quality Gate") {
        timeout(time: 1, unit: 'HOURS') {
            waitForQualityGate abortPipeline: true
        }
    } 
  
}
