pipeline {
  agent {
    node {
      label 'kaniko'
    }

  }
  stages {
    stage('Build and Publish DB') {
      steps {
        container(name: 'kaniko') {
          sh '''echo \'{ "credsStore": "ecr-login" }\' > /kaniko/.docker/config.json
/kaniko/executor -f `pwd`/compose/Dockerfile.db -c `pwd` --insecure --skip-tls-verify --cache=false --destination=${ECR_REPO}:orderbookdb-dev-42'''
        }

      }
    }
    stage('Build and Publish API') {
      steps {
        container(name: 'kaniko') {
          sh '''echo \'{ "credsStore": "ecr-login" }\' > /kaniko/.docker/config.json
/kaniko/executor -f `pwd`/compose/Dockerfile.api -c `pwd` --insecure --skip-tls-verify --cache=false --destination=${ECR_REPO}:orderbookapi-dev-42'''
        }
      }
    }
    stage('Build Trading Client') {
      steps {
        container(name: 'kaniko') {
          sh '''echo \'{ "credsStore": "ecr-login" }\' > /kaniko/.docker/config.json
/kaniko/executor -f `pwd`/autoclient/Dockerfile.autoclient -c `pwd` --insecure --skip-tls-verify --cache=false --destination=${ECR_REPO}:orderbookac-dev-42'''
        }
      }
    }
  }
  environment {
    ECR_REPO = '108174090253.dkr.ecr.us-east-1.amazonaws.com/sre-course'
  }
  triggers {
    pollSCM('*/10 * * * 1-5')
  }
}
