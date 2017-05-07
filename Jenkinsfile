pipeline {
  agent any
  
  environment {
        DOCKER_HOST = '10.146.0.2:2375'
        DOCKER_COMPOSE_FILE = 'ci_cd/docker-compose.yml'
        DOCKER_REGISTRY = 'asia.gcr.io/my-project-01-157810'
        DOCKER_IMAGE = 'meteor-todos-app'
        VERSION = "1.0.${BUILD_NUMBER}"
        DOCKER_STACK = 'meteor-todos'
  }

  stages {
    stage("Ready") {
      steps {
        sh "/usr/local/bin/meteor npm install"
      }
    }
    stage("Unit") {
      steps {
        echo "Unit testing phase."
      }
    }
    stage("Integ") {
      steps {
        echo "Integration testing phase."
        sh "/usr/local/bin/meteor test --once --driver-package dispatch:mocha"
      }
    }
    stage("Publish") {
      steps {
        sh "/usr/local/bin/docker-compose -f ${DOCKER_COMPOSE_FILE} build app"
        sh "docker tag ${DOCKER_IMAGE} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}"
        sh "docker tag ${DOCKER_IMAGE} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${VERSION}"
        sh "gcloud docker -- push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}"
        sh "gcloud docker -- push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${VERSION}"
      }
    }
    stage("Prod-like") {
      steps {
        echo "A production-like cluster is yet to be created"
      }
    }
    stage("Production") {
      steps {
        echo "Producton phase."
        sh "docker service update --image ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${VERSION} ${DOCKER_STACK}_app"
      }
    }
  }
  
  post {
    always {
        echo "This pipeline has finished."
        deleteDir()
    }
  }
}
