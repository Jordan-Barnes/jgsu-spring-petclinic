/* groovylint-disable CompileStatic */
pipeline {
    agent any

    triggers {
        pollSCM('* * * * *')
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/Jordan-Barnes/jgsu-spring-petclinic.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh './mvnw test'
            }
            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
                changed {
                        /* groovylint-disable-next-line GStringExpressionWithinString */
                    emailext subject: "Job \'${JOB_NAME}\' (${BUILD_NUMBER}) ${currentBuild.result}",
                        /* groovylint-disable-next-line GStringExpressionWithinString */
                        body: "Please go to ${BUILD_URL} and verify the build",
                        attachLog: true,
                        compressLog: true,
                        recipientProviders: [upstreamDevelopers(), requestor()]
                }
            }
        }
    }
}
