pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                // Pulls down the code from your repository
                checkout scm
            }
        }

        stage('Generate Report') {
            steps {
                echo 'Generating report...'
                // Native Windows command creating a text report since generate_report.bat is missing
                bat '''
                echo === Execution Report === > build_report.txt
                echo Job Name: %JOB_NAME% >> build_report.txt
                echo Build Number: %BUILD_NUMBER% >> build_report.txt
                echo Workspace Location: %WORKSPACE% >> build_report.txt
                echo Date and Time: %DATE% %TIME% >> build_report.txt
                echo Python Application Check: >> build_report.txt
                
                if exist app.py (
                    echo SUCCESS: app.py found in directory. >> build_report.txt
                ) else (
                    echo WARNING: app.py missing from workspace directory. >> build_report.txt
                )
                
                type build_report.txt
                '''
            }
        }

        stage('Archive Report') {
            steps {
                echo 'Archiving build artifacts...'
                // Saves the generated report file into Jenkins build storage
                archiveArtifacts artifacts: 'build_report.txt', allowEmptyArchive: false
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline execution finished.'
        }
    }
}
