node {


   stage('Validation') {
        echo "validation"

        username="unknown"
        wrap([$class: 'BuildUser']) {
            username = "${BUILD_USER}"
        }
        echo "user who built the code is $username"
    }

    stage('build') {
        echo "build" 

        checkout scm
        sh("ls ")
        checkout([$class: 'GitSCM']) {
            branchanme = "${GIT_BRANCH}"
            currentGitCommit = "${GIT_COMMIT}"
            previousGitCommit = "${GIT_PREVIOUS_SUCCESSFUL_COMMIT}"
        }
        
        echo "branchanme $branchanme"
        echo "currentGitCommit $currentGitCommit"
        echo "previousGitCommit $previousGitCommit"
        echo "Latest commit id : "
        sh("git log -n 1")
        


    }

    stage('apply') {
        echo "apply"   
    }
}