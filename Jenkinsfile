pipeline {
    agent { label 'UBUNTU_NODE2'}
    triggers { pollSCM('* * * * *')}
    stages {
        stage('vcs'){
            steps {
                git url: 'https://github.com/srikanthvelma/spring-petclinic.git',
                    branch: 'jfrog'
            }
        }
        stage('Artifactory config') {
            steps {
                rtServer (
                    id: "ARTIFACTORY_SERVER",
                    url: 'https://srikanthvelma.jfrog.io/artifactory',
                    credentialsId: 'JFROG_KEY'
                )
                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )
                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: 'libs-release',
                    snapshotRepo: 'libs-snapshot'
                )
            }
        }
        stage('build') {
            steps {
                rtMavenRun (
                    tool: 'MAVEN_8',
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER"
                )
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
        stage('sonar analysis') {
            steps {
                withSonarQubeEnv('SONARQUBE_CLOUD') {
                    sh 'mvn clean verify sonar:sonar \
                        -Dsonar.login=0953f64b8781d5a206154146dc6415dda632c5da\
                        -Dsonar.organization=springpetclinic57\
                        -Dsonar.projectKey=springpetclinic57_petclinic1'
                }
            }
        }
        stage('post build') {
            steps {
                archiveArtifacts artifacts: '**/target/spring-petclinic-3.0.0-SNAPSHOT.jar',
                                 onlyIfSuccessful: true
                junit testResults: '**/surefire-reports/TEST-*.xml'
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