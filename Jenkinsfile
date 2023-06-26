pipeline {
    agent any

    triggers {
        githubPush()
    }

    options {
        buildDiscarder(logRotator(numToKeepStr:'10', daysToKeepStr:'28', artifactNumToKeepStr:'5', artifactDaysToKeepStr:'14'))
        githubProjectProperty(displayName: 'FairServices', projectUrlStr: 'https://github.com/TheExkaliburg/FairServices.git/')
    }
    
    parameters {
        booleanParam(description: 'Restarts the WikiJS Server even if there are no new changes', name: 'WikiJS', defaultValue:'false')
    }

    environment {
        ENV_FILE = credentials('fairwiki-env')
    }

    stages {
        /*
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/TheExkaliburg/FairServices.git'
            }
        }
        */
        
        stage('FairWiki') {
            when { 
                anyOf {
                    expression { params.WikiJS }
                    changeset 'fairwiki/**'
                }
            } 
            steps {
                dir('fairwiki') {
                    sh 'sudo docker compose stop'
                    sh 'sudo docker compose --env-file $ENV_FILE up --pull always --force-recreate --detach'
                }
            }
        }
    }
}
