// Bitbucket
def GIT_CRED_ID = 'BitBucket'

// Constants
def BUILD_NUMBER = env.BUILD_NUMBER
def SANDBOX_HOST = 'https://test.salesforce.com'
def PROD_HOST = 'https://login.salesforce.com'
	
// TP6
def TP6_JWT_KEY_CRED_ID = "JWT_KEY_CRED_ID"
def TP6_CONNECTED_APP_CONSUMER_KEY = "3MVG96_7YM2sI9wTzbkE1GyltZyU1cm_R.IQ1yxHNM2J5RvUW6puCVQ09F3OGdeNXe3Zpkk1UNpaU_q2WlhSw"
def TP6_HOST = PROD_HOST
def TP6_USERNAME = 'hamza.hamoumi@curious-panda-90d3bp.com'
def TP6_ENCRYPTEDPASSWORD = 'yourEncryptedPassword' 			// Needed if you use Data Loader Command Line only!
def TP6_ENABLE_DATALOADER = false								// Enable Data Loader Commandline when deploying to this sandbox
def TP6_ORGNAME = 'TP6'

node {
   def mvnHome
   def toolbelt = tool 'toolbelt'

   // Clone the repository
	stage('Clone') {
		// Retrieve source
		checkout scm

		// Determine the SCRM url so we can push tags later on
		SCM_URL = bat(returnStdout: true, script: 'git config remote.origin.url').trim()
		echo "${SCM_URL}"
	}
   
   stage('Push To TP6') {
		withCredentials([file(credentialsId: TP6_JWT_KEY_CRED_ID, variable: 'orgSpecificJwtCredId')]) {
           	rc = bat returnStatus: true, script: "echo ${orgSpecificJwtCredId}"
           	rc = bat returnStatus: true, script: "sfdx force"
       
            echo "authenticating"
        	rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${TP6_CONNECTED_APP_CONSUMER_KEY} --username ${TP6_USERNAME} --jwtkeyfile \"${orgSpecificJwtCredId}\" --instanceurl ${TP6_HOST} --loglevel debug"
			rc = bat returnStatus: true, script: "sfdx force:alias:list"
			rc = bat returnStatus: true, script: "sfdx force:org:list"

			echo "deploying"
			// Deploy the converted code
			def checkOnly = false
			def checkOnlyArg = checkOnly? '--checkonly' : ''

			rmsg = bat returnStdout: true, returnStatus: false, script: "sfdx force:mdapi:deploy --targetusername ${TP6_USERNAME} --deploydir .\\force-app\\main\\default --testlevel RunLocalTests --json ${checkOnlyArg} --ignorewarnings"
			rmsg = rmsg.split('\n')[2].trim()
		}
   }
}



/***********************************************************************************************************************
 * Stage specific methods
 **********************************************************************************************************************/

/**
 * Deploys traditional metadata files to a conventional sandbox.
 *
 * @param toolbelt 			Path to where the SFDX tooling is installed.
 * @param clientId 			The Connected App client ID which will be used for logging into that sandbox.
 * @param username			The Username of the User jenkins is using to build to target environment.
 * @param encryptedpass		The Password of the User jenkins is using to load via Data Loader Command Line.
 * @param jwtkeyfile 		The path to the certificate which will be used to sign the JWT token when logging into the sandbox.
 * @param instanceurl  		The host name to use to log into Salesforce (e.g. https://login.salesforce.com)
 * @param orgName  			Name of the sandbox or Production environment
 * @param checkOnly     	Set to true if the deployment should be check only deployment
 */
def deployMetadata(toolbelt, clientId, username, encryptedpass, jwtkeyfile, instanceurl, orgName, checkOnly) {

	// Login into the sandbox
	rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${clientId} --username ${username} --jwtkeyfile ${jwtkeyfile} --instanceurl ${instanceurl} --loglevel debug"

	if (rc != 0) {
		error 'Failed to login into the org'
	}
    /*
	// Deploy the converted code
	def checkOnlyArg = checkOnly? '--checkonly' : ''

	if(isUnix()){
		rmsg = sh returnStdout: true, returnStatus: false, script: "${toolbelt}/sfdx force:mdapi:deploy --targetusername ${username} --deploydir force-app\\main\\default --testlevel RunLocalTests --json ${checkOnlyArg} --ignorewarnings"
	} else {
		rmsg = bat returnStdout: true, returnStatus: false, script: "sfdx force:mdapi:deploy --targetusername ${username} --deploydir force-app\\main\\default --testlevel RunLocalTests --json ${checkOnlyArg} --ignorewarnings"
		rmsg = rmsg.split('\n')[2].trim()
	}

	def orgDetails;

	if(isUnix()){
		orgDetails = sh returnStdout: true, returnStatus: false, script: "${toolbelt}/sfdx force:org:display --targetusername ${username} --json"
	} else {
		orgDetails = bat returnStdout: true, returnStatus: false, script: "sfdx force:org:display --targetusername ${username} --json"
		orgDetails = orgDetails.split('\n')[2].trim()
	}
    */
}