pipeline {
    agent any
    stages {
        stage('Restore') {
            when {
                branch 'dev'
            }
            steps {
                sh "dotnet restore"
            }
        }
        stage('Build') {
            when {
                branch 'dev'
            }
            steps {
                sh "dotnet build"
                sh "dotnet publish -c release -o /var/lib/jenkins/workspace/netcoreApi/output/dev"
            }
        }
        stage('Test') {
            when {
                branch 'dev'
            }
            steps {
                sh "dotnet test "
                sh "echo Test completed"
            }
        }
        stage('Deploy') {
            when {
                branch 'dev'
            }
            steps {
                sh "echo Deploy complete"
                sh "rsync -a /var/lib/jenkins/workspace/netcoreApi/output/dev sadeed@127.0.0.1:/home/sadeed/workspace/pracs/dotnetJenkins/netcore/dev"
            }
        }
        stage('Restart') {
            when {
                branch 'dev'
            }
            steps {
                sh "echo restart service"
                sh "sudo systemctl restart netcore-api.service"
            }
        }
    }
}