pipeline{

    environment{
        gitUserEmail = "jenkins-ci@lloydsbanking.com"
        gitUserName = "Jenkins-CI"
    }
    agent any
    stages{
        stage('Stage : Get branch name and push tag'){
            steps{
                script{
                    if (env.BRANCH_NAME == "master" ) {
                        initialise()
                        pushTag()
                    }
                }
            }
        }

        stage('tag building'){
            when { buildingTag() }    
            steps{
                script{
                    sh "echo ${env.TAG_NAME}"
                }
            }
        }
    }
}

def initialise(){

	withCredentials([usernamePassword(credentialsId: 'personal-pat', passwordVariable: 'C_PASSWORD', usernameVariable: 'C_USERNAME')]) {
		def tokenisedOrigin = "https://"+C_USERNAME+":"+C_PASSWORD+"@"+GIT_URL.split("https://")[1]
		sh("git remote set-url origin ${tokenisedOrigin}")
		sh("git config  user.email \"${gitUserEmail}\"")
        sh("git config  user.name \"${gitUserName}\"")
	}

	// Setting the proxy
	//sh("git config http.proxy \"${gitHttpProxy}\"")
	// Force used to refresh local tags
	sh("git fetch --tags --force")
	def gitTagSplit = sh(returnStdout: true, script: "git tag --sort version:refname | tail -1").trim().tokenize(".")
	gitTagSplit = gitTagSplit ?: ["v0", "0", "0"]

	gitTag = [gitTagSplit[0], {gitTagSplit[1].toInteger() + 1}().toString(), gitTagSplit[2]].join(".")
}

def pushTag(){
	try {
		sh("git describe --exact-match HEAD")
		println("[SKIPPING] Tag is currently matches HEAD - no need to push tag")
	} catch(e){
		withCredentials([usernamePassword(credentialsId: 'personal-pat', passwordVariable: 'C_PASSWORD', usernameVariable: 'C_USERNAME')]) {
			def tokenisedOrigin = "https://"+C_USERNAME+":"+C_PASSWORD+"@"+GIT_URL.split("https://")[1]
			sh("git remote set-url origin ${tokenisedOrigin}")
			sh("git config user.email \"${gitUserEmail}\"")
		}
		// Setting the proxy
		//sh("git config http.proxy \"${gitHttpProxy}\"")

		sh("git tag -am \"Docker build - ${gitTag}\" ${gitTag} && git push -u origin ${gitTag}")
		println("Tag generated: ${gitTag}")
	}
}