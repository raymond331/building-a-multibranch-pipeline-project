pipeline {
    agent any
    stages {

        stage('Tag branch') {

            env.develop_version = 'v1.0.0-dev.$BUILD_NUMBER'
            env.release_version = 'v1.0.0-rc.$BUILD_NUMBER'

            when {
                branch 'develop'
            }
            steps {
                sh 'git tag -a $develop_version -m "adding tag"'
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
                sh 'git tag -a $env.release_version -m "adding tag"'
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
                sh 'mv ./build/libs/led-app.war ./build/libs/led-app-$develop_version.war'
            }
            when {
                branch 'release/1.0.0'
            }
            steps {
                sh 'gradle clean -Penv=prod war'
                sh 'mv ./build/libs/lead-app.war ./build/libs/led-app-"${release_version}".war'
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

