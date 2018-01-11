pipeline {
    agent any
    stages {
        stage('Tag develop') {
          when {
              branch 'develop'
          }
          steps {
              sh 'git tag -a v1.0.0-dev.$BUILD_NUMBER -m "adding tag"'
              sh 'git push origin --tags'
          }
        }
        stage('Tag release') {
            when {
                branch 'release/1.0.0'
            }
            steps {
              sh 'git config --global user.email raymond@outlook.com'
              sh 'git config --global user.name raymond'
              sh 'git tag -a v1.0.0-rc.$BUILD_NUMBER -m "adding tag"'
              sh 'git push origin --tags'
            }
        }
    }
}
