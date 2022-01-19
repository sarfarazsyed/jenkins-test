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
        //echo sendChangeLogs()

        passedBuilds = []
        lastSuccessfulBuild(passedBuilds, currentBuild)
        echo "$passedBuilds"
        getChangedFiles(passedBuilds)
    }

    stage('apply') {
        echo "apply"   
    }
}

@NonCPS
def lastSuccessfulBuild(passedBuilds, build) {
  if ((build != null) && (build.result != 'SUCCESS')) {
      passedBuilds.add(build)
      lastSuccessfulBuild(passedBuilds, build.getPreviousBuild())
   }
}
 
def getChangedFiles(passedBuilds) {
    def files = [] as Set
    for (int h = 0; h < passedBuilds.size(); h++) {
        def changeLogSets = passedBuilds[h].changeSets
        echo " change log sets ${h} : ${changeLogSets} \n"
        for (int i = 0; i < changeLogSets.size(); i++) {
            def items = changeLogSets[i].items
            for (int j = 0; j < items.size(); j++) {
                def item = items[j]
                echo "Commit Information \nAuhor Name: $item.authorName\nCommit ID: $item.commitId\nTimestamp : formatter.format(new Date($item.timestamp))}\nCommit Message : *$item.msg*\n"
                def modifiedFiles = item.affectedFiles
                for (int k = 0; k < modifiedFiles.size(); k++) {
                    files.add(modifiedFiles[k].path)
                }
            }
        }
    }
    
    def filesInSortedOrder=files.toSorted()
    echo getModifiedFilesString(filesInSortedOrder)
    return filesInSortedOrder
}

def printProperties(item) {
    item.properties
    .each { 
        print "$it.key -> $it.value \t" 
    }
}

def getModifiedFilesString(array) {
    def msg = "Changed files: "
    array
    .each { 
        msg = msg+ "\n\t|\n\t|->$it" 
    }
    return msg
}