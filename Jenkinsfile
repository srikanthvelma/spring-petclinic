pipeline {
    agent { label 'UBUNTU_NODE2'}
    triggers { pollSCM('* * * * *')}
    stages {
        stage('vcs'){
            steps {
                git url: 'https://github.com/srikanthvelma/spring-petclinic.git',
                    branch: 'declarative'
            }
        }
        stage('build') {
            steps {
                sh 'mvn package'
            }
        }
        stage('sonar analysis') {
            steps {
                withSonarQubeEnv('SONAQUBE_CLOUD') {
                    sh 'mvn clean verify sonar:sonar\
                        -Dsonar.login=0953f64b8781d5a206154146dc6415dda632c5da\ 
                        -Dsonar.organization=springpetclinic57\
                        -Dsonar.projectKey=springpetclinic57_petclinic1'
                }
            }
        }
        stage('copy build') {
            steps{
                sh "mkdir -p /tmp/archive/${JOB_NAME}/${BUILD_ID} && cp ./target/spring-petclinic-*.jar /tmp/archive/${JOB_NAME}/${BUILD_ID}/"
                sh "aws s3 sync /tmp/archive/${JOB_NAME}/${BUILD_ID} s3://srikanthcicd"
            }
        }
        stage('postbuild') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar'
                                 junit '**/surefire-reports/TEST-*.xml'
            }
        }
    }
    post {
        success {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is success",
                 body: "Use this URL ${BUILD_URL} for more info",
                 to: 'team-all-qt@qt.com',
                 from: 'devops@qt.com'
        }
        failure {
            mail subject: "Jenkins Build of ${JOB_NAME} with id ${BUILD_ID} is failed",
                 body: "Use this URL ${BUILD_URL} for more info",
                 to: 'team-all-qr@qt.com',
                 from: 'devops@qt.com'
        }
    }
}