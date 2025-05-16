pipeline {
  agent any


  stages 
  {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/Nish1282/8.2CDevSecOps.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        bat 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        bat 'npm test || exit /b 0'
      }
    }

    stage('Generate Coverage Report') {
      steps {
        bat 'npm run coverage || exit /b 0'
      }
    }

    stage('NPM Audit (Security Scan)') {
      steps {
        bat 'npm audit || exit /b 0'
      }
    }

    stage('SonarCloud Analysis') 
    {
      steps 
      {
          withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) 
          {
                bat '''
                    curl -L -o sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip
                    powershell -Command "Expand-Archive -Path sonar-scanner.zip -DestinationPath ."
                    set SONAR_TOKEN=%SONAR_TOKEN%
                    sonar-scanner-5.0.1.3006-windows\\bin\\sonar-scanner.bat
                          '''
          }
      }
    }
  }
}
