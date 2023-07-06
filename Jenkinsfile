pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'dev', url: 'https://github.com/AjayPulapa/spring-jaco-code-covarge-check.git'
            } 
        }
             stage('Code Coverage Check') {
            steps {
                script {
                    def codeCoverage = getCoverage()
                    echo "Code Coverage: ${codeCoverage}%"
                    if (codeCoverage > 60) {
                        echo "Code coverage is above 60%. Proceeding with the pipeline."
                    } else {
                        error("Code coverage is below 60%. Aborting pipeline.")
                    }
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed.'
            // Add any additional actions or notifications for failure cases
        }
    }
}
def getCoverage() {
    bat 'mvn clean test jacoco:report'

    def coverageReport = readFile('target/site/jacoco/index.html')
    def matcher = coverageReport =~ /<span class="percentage">(\d+)%<\/span>/
    def coveragePercentage = matcher[0][1].toInteger()

    return coveragePercentage
}






