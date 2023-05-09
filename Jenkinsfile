pipeline {
    
	agent any
	tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }
	
    environment {
        SNAP_REPO = "amine-snapshot"
        NEXUS_USER = "admin"
        NEXUS_PASS = "admin"
        RELEASE_REPO = "amine-release"
        CENTRAL_REPO = "amine_maven_central"
        NEXUSIP = "18.134.210.152"
        NEXUSPORT = "8081"
	    NEXUL_GRP_REPO = "amine-maven-group"
        NEXUS_LOGIN = "nexuslogin"
        ARTVERSION = "${env.BUILD_ID}"
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
    }
    }
    


}
