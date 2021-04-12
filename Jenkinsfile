pipeline{
    agent any
    triggers { pollSCM('* * * * *') }
    stages{
        stage("Checkout"){
            steps{
                echo "======== Executing echckout - git clone ========"
                git branch: 'main', url: 'https://github.com/migorsage/jgsu-spring-petclinic.git'
            }
        }
        stage("Build") {
            steps {
                sh "./mvnw clean package"
            }
            post{
                always{
                    junit "**/target/surefire-reports/TEST-*.xml"
                    archiveArtifacts "target/*.jar"
                }
                changed{
                    emailext attachLog: true, 
                    body: "Please go to ${BUILD_URL} and verify the build", 
                    compressLog: true, 
                    recipientProviders: [requestor(), upstreamDevelopers()],
                    to: 'test"jenkins',
                    subject: "Job \'${JOB_NAME}\' (${BUILD_NUMBER}) ${currentBuild.result}"
                }
            }
        }
    }
}