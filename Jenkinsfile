pipeline {
  agent any

  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/YOUR_USERNAME/8.2CDevSecOps.git'
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

    stage('SonarCloud Analysis') {
      steps {
        bat '''
          REM Clean up old sonar-scanner folder with PowerShell (more reliable)
          powershell -Command "Remove-Item -LiteralPath sonar-scanner-5.0.1.3006-windows -Force -Recurse -ErrorAction SilentlyContinue"
          
          REM Delete old sonar-scanner.zip if exists
          if exist sonar-scanner.zip del /f /q sonar-scanner.zip

          REM Download sonar-scanner CLI
          curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-windows.zip

          REM Extract sonar-scanner with force overwrite
          powershell -Command "Expand-Archive -Path sonar-scanner.zip -DestinationPath . -Force"

          REM Run sonar-scanner
          sonar-scanner-5.0.1.3006-windows\\bin\\sonar-scanner.bat
        '''
      }
    }
  }
}
