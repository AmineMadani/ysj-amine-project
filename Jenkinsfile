def COLOR_MAP = [
    'SUCCESS': 'good',
    'FAILURE': 'danger',
]
pipeline {
    agent any
    
    stages {        
        stage('Setup parameters') {
            steps {
                script {
                    properties([
                        parameters([
                            string(
                                defaultValue: '',
                                name: 'BUILD',
                            ),
                            string(
                                defaultValue: '',
                                name: 'TIME',
                            )
                        ])
                    ])
                }
                
        }
        }

        stage('Ansible Deploy to Production') {
            steps {
                ansiblePlaybook([
                inventory : 'ansible/prod.inventory',
                playbook : 'ansible/site.yml',
                installation : 'ansible',
                colorized : true,
                credentialsId : 'applogin-prod',
                disableHostKeyChecking: true,
                extraVars : [
                    USER: "admin",
                    PASS: "admin",
                    nexusip: "3.8.159.50",
                    reponame: "amine-release",
                    groupid: "QA",
                    time: "${env.TIME}",
                    build: "${env.BUILD}",
                    artifactid: "amine",
                    amine_version: "amine-${env.BUILD}-${env.TIMES}.war"

                ]
                ])
            }
        }
    }
    }
    post {
        always {
            echo 'Slack Notifications.'
            slackSend channel: '#jenkinscicd',
                color: COLOR_MAP[currentBuild.currentResult],
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
        }
    }


}