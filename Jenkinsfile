pipeline {
  agent any
  // triggers {
  //   cron('H/5 * * * *')
  //}

  environment {
        DOCKER_HOST = 'tcp://10.146.0.2:2375'
        DOCKER_STACK_NAME = 'meteor-todos'
	DOCKER_STACK_FILE = 'ci_cd/docker-compose.yml'
    }

  stages {
    stage("Build") {
      steps {
        echo "echo 'Build docker image...'"
      }
    }

    stage("Unit") {
      steps {
        echo "echo 'Unit testing...'"
      }
    }

    stage("Integ") {
      steps {
        echo "echo 'Integration testing...'"
      }
    }

    stage("Publish") {
      steps {
        echo "echo 'Publish image to registry...'"
      }
    }

    stage("Deploy") {
      steps {
        sh "docker stack deploy --compose-file ${DOCKER_STACK_FILE} ${DOCKER_STACK_NAME}"
      }
    }
  }

  post {
    always {
      echo "echo 'finished...'"
    }
  }
}
