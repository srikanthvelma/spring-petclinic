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
            tools {
                jdk 'JDK_17'
            }
            steps {
                sh 'gradle build'
            }
        }
        stage('postbuild') {
            steps {
                archiveArtifacts artifacts: '**/spring-petclinic-3.0.0.jar'
                                 junit '**/TEST-*.xml'
                                 mail subject: 'Build Failure',
                                      body: 'please find the details',
                                      to: 'velmasrikanth@gmail.com'
            }
        }
        
    }
}