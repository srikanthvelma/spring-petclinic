pipeline {
    agent { label 'UBUNTU_NODE2'}
    stages {
        stage('vcs'){
            steps {
                git url: 'https://github.com/srikanthvelma/spring-petclinic.git',
                    branch: 'gradle'
            }
        }
        stage('build') {
            steps {
                sh 'gradle build'
            }
        }
        stage('postbuild') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar'
                                 junit '**/surefire-reports/TEST-*.xml'
            }
        }
    }
}