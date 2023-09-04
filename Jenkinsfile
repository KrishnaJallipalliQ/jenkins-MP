pipeline {
    agent {
        label 'windows-latest'
    }
    
    environment {
        solution = '**/*.sln'
        buildPlatform = 'Any CPU'
        buildConfiguration = 'Release'
    }
    
    stages {
        stage('NuGet Tool Installer') {
            steps {
                bat 'nuget.exe'
            }
        }
        
        stage('NuGet Restore') {
            steps {
                bat "nuget restore ${solution}"
            }
        }
        
        stage('Build') {
            steps {
                bat "msbuild ${solution} /p:DeployOnBuild=true /p:WebPublishMethod=Package /p:PackageAsSingleFile=true /p:SkipInvalidConfigurations=true /p:PackageLocation=\"${env.BUILD_ARTIFACTSTAGINGDIRECTORY}\" /p:Platform=\"${buildPlatform}\" /p:Configuration=\"${buildConfiguration}\""
            }
        }
        
        stage('Test') {
            steps {
                bat "vstest.console.exe ${solution} /Platform:\"${buildPlatform}\" /Configuration:\"${buildConfiguration}\""
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
    }
}
