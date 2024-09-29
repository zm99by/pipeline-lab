pipeline {
  agent none
  stages {
    stage('Fluffy Build') {
      parallel {
        stage('Build Java 17') {
          agent {
            node {
              label 'java17'
            }

          }
          steps {
            sh './jenkins/build.sh'
            stash(name: 'Java 17', includes: 'target/**')
          }
        }

        stage('Build Node 1') {
          agent {
            node {
              label 'node1'
            }

          }
          steps {
            sh './jenkins/build.sh'
            archiveArtifacts 'target/*.jar'
            stash(name: 'Node 1', includes: 'target/**')
          }
        }

      }
    }

    stage('Fluffy Test') {
      parallel {
        stage('Backend Java 17') {
          agent {
            node {
              label 'java17'
            }

          }
          steps {
            unstash 'Java 17'
            sh './jenkins/test-backend.sh'
            junit 'target/surefire-reports/**/TEST*.xml'
          }
        }

        stage('Frontend Java 17') {
          agent {
            node {
              label 'java17'
            }

          }
          steps {
            unstash 'Java 17'
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
          }
        }

        stage('Performance Java 17') {
          agent {
            node {
              label 'java17'
            }

          }
          steps {
            unstash 'Java 17'
            sh './jenkins/test-performance.sh'
          }
        }

        stage('Static Java 17') {
          agent {
            node {
              label 'java17'
            }

          }
          steps {
            unstash 'Java 17'
            sh './jenkins/test-static.sh'
          }
        }

        stage('Backend Node 1') {
          agent {
            node {
              label 'node1'
            }

          }
          steps {
            unstash 'Node 1'
            sh './jenkins/test-backend.sh'
            junit 'target/surefire-reports/**/TEST*.xml'
          }
        }

        stage('Frontend Node 1') {
          agent {
            node {
              label 'node1'
            }

          }
          steps {
            unstash 'Node 1'
            sh './jenkins/test-frontend.sh'
            junit 'target/test-results/**/TEST*.xml'
          }
        }

        stage('Performance Node 1') {
          agent {
            node {
              label 'node1'
            }

          }
          steps {
            unstash 'Node 1'
            sh './jenkins/test-performance.sh'
          }
        }

        stage('Static Node 1') {
          agent {
            node {
              label 'node1'
            }

          }
          steps {
            unstash 'Node 1'
            sh './jenkins/test-static.sh'
          }
        }

      }
    }

    stage('Confirm Deploy') {
      steps {
        input(message: 'Okay to deploy to staging?', ok: 'Yes')
      }
    }

    stage('Fluffy Deploy') {
      agent {
        node {
          label 'node1'
        }

      }
      steps {
        unstash 'Node 1'
        sh './jenkins/deploy.sh staging'
      }
    }

  }
}
