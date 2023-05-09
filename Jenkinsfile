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
        NEXUS_GRP_REPO = "amine-maven-group"
        NEXUS_LOGIN = "nexuslogin"
        SONARSERVER = "sonarserver"
        SONARSCANNER = "sonarscanner"
    }
	
    stages{
        
        stage('BUILD'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }
            post {
                success {
                    echo "Now Archiving."
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('UNIT TEST'){
            steps {
                sh 'mvn -s settings.xml test'
            }
        }

	    stage('INTEGRATION TEST'){
            steps {
                sh 'mvn -s settings.xml verify -DskipUnitTests'
            }
        }
		
        stage ('CODE ANALYSIS WITH CHECKSTYLE'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
            post {
                success {
                    echo 'Generated Analysis Result'
                }
            }
        }
        stage('CODE ANALYSIS with SONARQUBE') {
          
		  environment {
             scannerHome = tool "${SONARSCANNER}"
          }

          steps {
            withSonarQubeEnv("${SONARSERVER}") {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=amine \
                   -Dsonar.projectName=amine-repo \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }

    
          }
        }
    }
    


}
