pipeline {
   environment {
      develop_version = "v1.0.0-dev.$BUILD_NUMBER"
      release_version = "v1.0.0-rc.$BUILD_NUMBER"
      GIT_URL = "git@github.com:raymond331/building-a-multibranch-pipeline-project.git"
      GIT_USER = "raymond"
      GIT_EMAIL = "raymond.li331@outlook.com"
   }

    agent any
    stages {

        stage('Tag develop branch') {

            when {
                branch 'develop'
            }
            steps {
                sh 'git tag -a $develop_version -m "adding tag"'
                sh 'git remote set-url origin $GIT_URL'
                sshagent(['d0e645ce-ffa4-4fa1-95c3-a578c3f1eaf8']) {
                   sh 'git push origin --tags'
            }
        }
      }
         stage('Tag release branch') {
            when {
                branch 'release/1.0.0'
            }
            steps {
                sh 'git config --global user.email $GIT_EMAIL'
                sh 'git config --global user.name $GIT_USER'
                sh 'git tag -a $release_version -m "adding tag"'
                sh 'git remote set-url origin $GIT_URL'
                sshagent(['d0e645ce-ffa4-4fa1-95c3-a578c3f1eaf8']) {
                   sh 'git push origin --tags'
                }
            }
         }


        stage('Package develop') {
            when {
                branch 'develop'
            }
            steps {
                sh 'gradle clean -Penv=dev war'
                sh 'mv ./build/libs/led-app.war ./build/libs/led-app-$develop_version.war'
            }
        }

        stage('Package release') {
            when {
                branch 'release/1.0.0'
            }
            steps {
                sh 'gradle clean -Penv=prod war'
                sh 'mv ./build/libs/lead-app.war ./build/libs/led-app-$release_version.war'
            }
        }


        stage('Publish Develop Artifacts') {
            when {
                branch 'develop'
            }
            steps {
                sh 'echo "upload artifacts to Nexus..."'
            }
        }

        stage('Publish Publish Artifacts') {
            when {
                branch 'release/1.0.0'
            }
            steps {
                sh 'echo "upload artifacts to Nexus..."'
            }
        }
    }
}
