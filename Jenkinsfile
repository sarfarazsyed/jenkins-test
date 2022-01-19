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
        branchanme = "${env.BRANCH}"
        currentGitCommit = "${env.GIT_COMMIT}"
        previousGitCommit = "${env.GIT_PREVIOUS_SUCCESSFUL_COMMIT}"
        echo "branchanme $branchanme"
        echo "currentGitCommit $currentGitCommit"
        echo "previousGitCommit $previousGitCommit"
        branchnameNonEnv = "${BRANCH}"
        currentGitCommitNonEnv = "${GIT_COMMIT}"
        previousGitCommitNonEnv = "${GIT_PREVIOUS_SUCCESSFUL_COMMIT}"
        echo "branchnameNonEnv $branchnameNonEnv"
        echo "currentGitCommitNonEnv $currentGitCommitNonEnv"
        echo "previousGitCommitNonEnv $previousGitCommitNonEnv"
        


    }

    stage('apply') {
        echo "apply"   
    }
}