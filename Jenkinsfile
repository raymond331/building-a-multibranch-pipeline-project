pipeline {
   environment {
      develop_version = "v1.0.0-dev."
      release_version = "v1.0.0-rc."
   }

    agent any
    stages {

        stage('Tag develop branch') {

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
      }
         stage('Tag release branch') {
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
         }


        stage('Package develop') {
            when {
                branch 'develop'
            }
            steps {
                sh 'gradle clean -Penv=dev war'
                sh 'mv ./build/libs/led-app.war ./build/libs/led-app-"${env.develop_version}-${env.BUILD_NUMBER}".war'
            }
        }

        stage('Package release') {
            when {
                branch 'release/1.0.0'
            }
            steps {
                sh 'gradle clean -Penv=prod war'
                sh 'mv ./build/libs/lead-app.war ./build/libs/led-app-"${env.release_version}-${env.BUILD_NUMBER}".war'
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
