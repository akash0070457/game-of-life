node {
	
try {

      // Cleanup local checkout - TODO there should also be a dedicated jenkins command to invoke this action
    sh "rm -rf *"
    sh "rm -rf .git"
    // Clone from git
    checkout scm
    

	
    stage 'Set Version'
    def originalV = version();
    def major = originalV[0];
    def minor = originalV[1];
    def v = "${major}.${minor}-${env.BUILD_NUMBER}"
    if (v) {
       echo "Building version ${v}"
    }
    sh "/opt/maven/bin/mvn -B versions:set -DgenerateBackupPoms=false -DnewVersion=${v}"
    
    /*stage 'Build'
         sh '/opt/maven/bin/mvn clean install -DskipTests -U' 
	*/
    stage 'Test'
      sh 'echo test'
	
    stage 'Deploy'

      echo 'Push to Repo'
     
    stage 'Cleanup'
        deleteDir()
      echo 'prune and cleanup'
      // echo sh(returnStdout: true, script: 'env')

      
      mail body: "${env.JOB_NAME}-${env.BUILD_NUMBER} build successful, Please go to ${env.BUILD_URL}.",
        from: 'build@localhost',
        replyTo: 'build@localhost',
        subject: "${env.JOB_NAME}-${env.BUILD_NUMBER} build successful",
        to: "srinivasa.pashupathi@gmail.com"
		}

  catch (err) {
    currentBuild.result = "FAILURE"

   
    mail body: "${env.JOB_NAME}-${env.BUILD_NUMBER} build error is here: ${env.BUILD_URL}, Please go to ${env.BUILD_URL}." ,
      from: 'build@localhost',
      replyTo: 'build@localhost',
      subject: "${env.JOB_NAME}-${env.BUILD_NUMBER} build failed",
      to: "srinivasa.pashupathi@gmail.com"
      throw err
    
  }
}
// Parse the pom.xml and extract the version information.
def version() {
    def matcher = readFile('pom.xml') =~ '<version>(.+)-.*</version>'
    matcher ? matcher[0][1].tokenize(".") : null
}
