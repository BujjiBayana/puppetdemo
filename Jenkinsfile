node {
  deleteDir()
  checkout scm
  echo 'beginnning workflow...'

  stage 'prepare gems'
  sh '''#!/bin/bash
  source ~/.rvm/scripts/rvm
  bundle install --path=.bundle/gems/
  '''

  stage 'syntax testing'
  sh '''#!/bin/bash
  source ~/.rvm/scripts/rvm
  bundle exec puppet parser validate manifests/
  '''

  stage 'lint testing'
  sh '''#!/bin/bash
  source ~/.rvm/scripts/rvm
  bundle exec bundle exec puppet-lint --no-autoloader_layout-check manifests/*.pp
  '''

  stage 'rspec testing'
  sh '''#!/bin/bash
  source ~/.rvm/scripts/rvm
  bundle exec rake spec
  '''

  stage 'smoke testing'
  sh '''#!/bin/bash
  source ~/.rvm/scripts/rvm
  bundle exec rake spec_prep
  puppet apply tests/init.pp --noop --modulepath=spec/fixtures/modules
  '''

  stage 'deploy'
  echo 'deploy to puppet masters'
}

checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '8a7f0152-3f35-47ef-a2ff-33ed65a8764d', url: 'https://github.com/BujjiBayana/puppetdemo.git']]])

