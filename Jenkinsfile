pipeline {
    
	agent any
    tools {
        maven "MAVEN3"
        jdk "JAVA_HOME"
    }
	
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUSPORT = "8081"
        NEXUSIP = "172.31.7.0"
        SNAP_REPO = 'vprofile-maven-snap'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = "vprofile-maven-central"
	    NEXUS_GRP_REPO = "vprofile-maven-group"
        NEXUS_LOGIN = "nexuslogin"
        
    }
	
    stages {
        stage('build'){
            steps{
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }
        post {
            success {
                echo "now archieved"
                archiveArtifacts artifacts: '**/*.war'
            }
        }
            steps{
                sh ''
            }
        }

        stage('Test'){
            steps {
                sh 'mvn test'
            }
        }

        stage('checkStyle'){
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        }
    }



