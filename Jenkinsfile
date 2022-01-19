import java.text.*
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
        // checkout(scm) {
        //     echo "GIT_COMMIT is ${env.GIT_COMMIT}"
        // }
            //echo "GIT_COMMIT is  $GIT_COMMIT"
            
        
        // checkout([$class: 'GitSCM']) {
        //     branchanme = "${GIT_BRANCH}"
        //     currentGitCommit = "${GIT_COMMIT}"
        //     previousGitCommit = "${GIT_PREVIOUS_SUCCESSFUL_COMMIT}"
        // }
        
        // echo "branchanme $branchanme"
        // echo "currentGitCommit $currentGitCommit"
        // echo "previousGitCommit $previousGitCommit"
        // echo "Latest commit id : "
        sh("git log -n 1")
        

        echo "changed commits 1"
        echo "changed commits 2"
        echo "changed commits 3"
        echo sendChangeLogs()

        passedBuilds = []
        lastSuccessfulBuild(passedBuilds, currentBuild)
        echo "$passedBuilds"
        echo getChangedFiles(passedBuilds)
    }

    stage('apply') {
        echo "apply"   
    }
}

@NonCPS
def sendChangeLogs() {
    def commitMessages = ""
    def formatter = new SimpleDateFormat('yyyy-MM-dd HH:mm')
    def changeLogSets = currentBuild.changeSets
    for (int i = 0; i < changeLogSets.size(); i++) {
        def entries = changeLogSets[i].items
        for (int j = 0; j < entries.length; j++) {
            def entry = entries[j]
            commitMessages = commitMessages + "${entry.author} ${entry.commitId}:\n${formatter.format(new Date(entry.timestamp))}: *${entry.msg}*\n" 
        }
    }
    return "Job: `${env.JOB_NAME}`. Starting build with changes:\n${commitMessages}"
}

@NonCPS
def lastSuccessfulBuild(passedBuilds, build) {
  if ((build != null) && (build.result != 'SUCCESS')) {
      passedBuilds.add(build)
      lastSuccessfulBuild(passedBuilds, build.getPreviousBuild())
   }
}
 
 @NonCPS
def getChangedFiles(passedBuilds, build) {
    def files = [] as Set // as Set assumes uniqueness
    passedBuilds.each {
        def changeLogSets = it.rawBuild.changeSets
        changeLogSets.each {
            it.items.each {
                it.affectedFiles.each {
                    files.add(it.path)
                }
            }
        }
    }
    echo "Found changes in files: ${files}"
    return files.toSorted()
}