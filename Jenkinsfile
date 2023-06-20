pipeline {
    
	agent any
    tools {
        maven "MAVEN3"
        jdk "JAVA_HOME"
    }
	
    environment {
        NEXUS_VERSION = "nexus3"
        NEXUS_PROTOCOL = "http"
        NEXUS_URL = "172.31.7.0:8081"
        NEXUS_REPOSITORY = "vprofile-maven-central"
	    NEXUS_REPOGRP_ID    = "vprofile-maven-group"
        NEXUS_CREDENTIAL_ID = "nexuslogin"
        ARTVERSION = "${env.BUILD_ID}"
    }
	
    stages {
        stage('build'){
            steps{
                sh 'mvn -s settings.xml -DskipTests install'
            }
        }

        }
    }



