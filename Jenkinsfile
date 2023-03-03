node('UBUNTU_NODE2') {
    stage('vcs') {
        git 'https://github.com/srikanthvelma/spring-petclinic.git'
    }
    stage('build') {
        sh 'mvn package'
    }
    stage('postbuild') {
        archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar',
        junit '**/surefire-reports/TEST-*.xml'
    }
}