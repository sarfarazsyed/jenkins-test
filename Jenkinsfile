node {


   stage('Validation') {
        echo "validation"

        username="unknown"
        wrap([$class: 'BuildUser']) {
            username = "${BUILD_USER}"
        }
        echo "$username"
    }

    stage('build') {
        echo "build" 

        checkout scm

        sh '${env.BRANCH}'
        echo "current git commit is ${GIT_COMMIT}"
        echo "previous git commit is ${GIT_PREVIOUS_SUCCESSFUL_COMMIT}"
        


    }

    stage('apply') {
        echo "apply"   
    }
}