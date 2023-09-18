pipeline {
    agent any
    
    stages {
        stage("build") {
            steps{
                sh "./mvnw package"
            }
        }

        stage("capture") {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', followSymlinks: false
                junit '**/target/surefire-reports/TEST*.xml'
                jacoco()
            }
        }
    }   
    
    post {
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}",
            to: 'test@test.onet.pl',
            recipientProviders: [previous()],
            subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
    }
    
}