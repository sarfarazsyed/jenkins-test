node {


   stage('Validation') {
        echo "validation"

        username="unknown"
        wrap([$class: 'BuildUser']) {
            username = "${BUILD_USER}"
        }
    }

    stage('build') {
        echo "build" 

        withCheckout(scm) {
            echo "current git commit is ${env.GIT_COMMIT}"
            echo "previous git commit is ${env.GIT_PREVIOUS_SUCCESSFUL_COMMIT}"
        }


    }

    stage('apply') {
        echo "apply"   
    }
}