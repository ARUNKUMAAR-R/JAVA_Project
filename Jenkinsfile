def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger',
]

pipeline {
    
	agent any
    tools {
        maven "maven3"
        jdk "openjdk11"
    }
	
    #environment {
        NEXUS_VERSION = "nexus3"
        NEXUSPORT = "8081"
        NEXUSIP = "172.31.7.124"
        SNAP_REPO = 'vprofile-maven-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = "vprofile-maven-release"
	    NEXUS_GRP_REPO = "vprofile-maven-group"
        NEXUS_LOGIN = "nexus-login"
        SONARSERVER = 'SonarServer'
        SONARSCANNER = 'SonarScanner'
        
    }
	
    stages {
        stage('build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
        
            post {
                success {
                    echo "now archieved"
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }            

        stage('Test'){
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

        stage('checkStyle'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }

     
        stage('Sonar Analysis'){
            environment {
                scannerHome = tool "${SONARSCANNER}"
            }
            steps {
                withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''

            }
        }
    }
        stage("UploadArtifact"){
            steps{
                nexusArtifactUploader(
                  nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                  groupId: 'QA',
                  version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                  repository: "${RELEASE_REPO}",
                  credentialsId: "${NEXUS_LOGIN}",
                  artifacts: [
                    [artifactId: 'vprofile-tmp',
                     classifier: '',
                     file: 'target/vprofile-v2.war',
                     type: 'war']
                  ]
                )
            }
        }
    }
    post {
        always {
            echo 'Slack Notifications.'
            slackSend channel: '#vprofile-app',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }
    
}




