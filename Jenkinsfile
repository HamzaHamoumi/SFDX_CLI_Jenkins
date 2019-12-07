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
   
   stage('Push To TP6') {
        withCredentials([file(credentialsId: TP6_JWT_KEY_CRED_ID, variable: 'orgSpecificJwtCredId')]) {
           rc = bat returnStatus: true, script: "echo ${orgSpecificJwtCredId}"
           rc = bat returnStatus: true, script: "sfdx force"
       }
   }
}