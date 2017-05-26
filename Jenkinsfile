properties([
    [$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', numToKeepStr: '20']],
    [$class: 'GithubProjectProperty', projectUrlStr: 'https://github.com/coreos/matchbox'],
    [$class: 'PipelineTriggersJobProperty', triggers: [
      [$class: 'GitHubPushTrigger'],
    ]]
])
parallel (
  etcd3: {
    node('fedora && bare-metal') {
      stage('etcd3') {
        timeout(time:5, unit:'MINUTES') {
          checkout scm
          sh '''#!/bin/bash -e
          export ASSETS_DIR=~/assets; ./tests/smoke/etcd3
          '''
        }
      }
    }
  },
  bootkube: {
    node('fedora && bare-metal') {
      stage('bootkube') {
        timeout(time:12, unit:'MINUTES') {
          deleteDir()
          checkout scm          
          sh '''#!/bin/bash -e
          chmod 600 ./tests/smoke/fake_rsa
          export ASSETS_DIR=~/assets; ./tests/smoke/bootkube
          '''
        }
      }
    }
  },
  "etcd3-terraform": {
    node('fedora && bare-metal') {
      stage('etcd3-terraform') {
        timeout(time:10, unit:'MINUTES') {
          checkout scm
          sh '''#!/bin/bash -e
          export ASSETS_DIR=~/assets; export CONFIG_DIR=~/matchbox/examples/etc/matchbox; ./tests/smoke/etcd3-terraform
          '''
        }
      }
    }
  },
  "bootkube-terraform": {
    node('fedora && bare-metal') {
      stage('bootkube-terraform') {
        timeout(time:15, unit:'MINUTES') {
          deleteDir()
          checkout scm
          sh '''#!/bin/bash -e
          export ASSETS_DIR=~/assets; export CONFIG_DIR=~/matchbox/examples/etc/matchbox; ./tests/smoke/bootkube-terraform
          '''
        }
      }
    }
  },
)
