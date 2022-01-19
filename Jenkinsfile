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
        

        echo "Modified commits 1"
        echo "Modified commits 2"
        echo "Modified commits 3"
        //echo sendChangeLogs()

        passedBuilds = []
        lastSuccessfulBuild(passedBuilds, currentBuild)
        echo "$passedBuilds"
        files = getModifiedFiles(passedBuilds)
    }

    stage('apply') {
        sh("${files} | grep 'dev' | sed 's#/# #g' | awk '{print $3\" ./config/configmap/\"$3\"/dev.yaml\";}")
        //sh('''${files} | grep 'dev' | sed 's#/# #g' | awk '{print $2" ./"$2"/dev.yaml";}' | awk '{ $kubeDevCmd create configmap $1 --from-file=$2 -o yaml --dry-run | kubectl replace -f - }' ''')
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
 
def getModifiedFiles(passedBuilds) {
    def files = [] as Set
    for (int h = 0; h < passedBuilds.size(); h++) {
        def changeLogSets = passedBuilds[h].changeSets
        echo " change log sets ${h} : ${changeLogSets} \n"
        for (int i = 0; i < changeLogSets.size(); i++) {
            def items = changeLogSets[i].items
            for (int j = 0; j < items.size(); j++) {
                def item = items[j]
                def commitMsg = "Commit Information \nAuhor Name: $item.author\nCommit ID: $item.commitId\nTimestamp : formatter.format(new Date($item.timestamp))}\nCommit Message : *$item.msg*\nModified files: "
                def modifiedFiles = item.affectedFiles
                for (int k = 0; k < modifiedFiles.size(); k++) {
                    def path = modifiedFiles[k].path
                    commitMsg = commitMsg + "\n\t|\n\t|-> ${modifiedFiles[k].path}"
                    if(path.contains("config/configmap")) {
                        files.add(path)
                    }
                }
                echo commitMsg
            }
        }
    }
    def filesInSortedOrder=files.toSorted()
    echo getModifiedFilesString(filesInSortedOrder)
    return filesInSortedOrder
}

def getModifiedFilesString(array) {
    def msg = "Modified Config files: "
    array
    .each { 
        msg = msg+ "\n\t|\n\t|->$it" 
    }
    return msg
}