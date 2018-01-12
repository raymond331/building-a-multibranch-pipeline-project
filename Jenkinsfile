pipeline {
    agent any
    stages {

        stage('Tag branch') {

            env.develop_version = 'v1.0.0-dev.'
            env.release_version = 'v1.0.0-rc.'

            when {
                branch 'develop'
            }
            steps {
                sh 'git tag -a "${env.develop_version}-${env.BUILD_NUMBER}" -m "adding tag"'
                sh 'git remote set-url origin git@github.com:raymond331/building-a-multibranch-pipeline-project.git'
                sshagent(['d0e645ce-ffa4-4fa1-95c3-a578c3f1eaf8']) {
                   sh 'git push origin --tags'
            }
        }
            when {
                branch 'release/1.0.0'
            }
            steps {
                sh 'git config --global user.email raymond@outlook.com'
                sh 'git config --global user.name raymond'
                sh 'git tag -a "${env.release_version}-${env.BUILD_NUMBER}" -m "adding tag"'
                sh 'git remote set-url origin git@github.com:raymond331/building-a-multibranch-pipeline-project.git'
                sshagent(['d0e645ce-ffa4-4fa1-95c3-a578c3f1eaf8']) {
                   sh 'git push origin --tags'
                }
            }

        stage('Package') {
            when {
                branch 'develop'
            }
            steps {
                sh 'gradle clean -Penv=dev war'
                sh 'mv ./build/libs/led-app.war ./build/libs/led-app-"${env.develop_version}-${env.BUILD_NUMBER}".war'
            }
            when {
                branch 'release/1.0.0'
            }
            steps {
                sh 'gradle clean -Penv=prod war'
                sh 'mv ./build/libs/lead-app.war ./build/libs/led-app-"${env.release_version}-${env.BUILD_NUMBER}".war'
            }

        stage('Publish Artifacts')
            when {
                branch 'develop'
            }
            steps {
                sh 'echo "upload artifacts to Nexus..."'
            }
            when {
                branch 'release/1.0.0'
            }
            steps {
                sh 'echo "upload artifacts to Nexus..."'
            }
        }
    }
  }
}

