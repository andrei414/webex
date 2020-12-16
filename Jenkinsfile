#!groovy
 
import groovy.json.JsonSlurperClassic

node {
	
    withEnv(["HOME=${env.WORKSPACE}"]) {
        withEnv(["PATH+sfx=${WORKSPACE}/sfdx/bin"]) {
            stage('Install Sfdx') {
                sh '''
                    if [ -x sfdx/bin/sfdx ]; then exit 0; fi
                    mkdir -p sfdx
                    curl -sS -o sfdx.tar.xz https://developer.salesforce.com/media/salesforce-cli/sfdx-linux-amd64.tar.xz
                    ls -l sfdx.tar.xz
                    tar xJf sfdx.tar.xz -C sfdx --strip-components 1
                '''
                sh 'echo WORKSPACE=${WORKSPACE}'
                sh 'echo $PATH'
                sh 'sfdx --help'
            }
			
			stage("set env variable"){
				env.HUB_ORG="conttest414-aad2@force.com"
				env.SFDC_HOST="https://test-3b7.my.salesforce.com/"
				env.CONNECTED_APP_CONSUMER_KEY="3MVG9SOw8KERNN0.kF.gZhK.3VoVL65c2VncoLTiigo1vv50w4m8GQiUw5VoIURBHzBvmt9W_u_NyvEM7ES78"
				env.JWT_KEY_CRED_ID="fc627bd8-4ca9-4b40-92e4-d7cebb6ef642"
			}
			
			stage('checkout source') {
        		// when running in multi-branch job, one must issue this command
        		checkout scm
    		}

			withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
				// -------------------------------------------------------------------------
				// Deploy (new optional)
				// -------------------------------------------------------------------------
				stage('Deploy Code') {
					println 'KEY IS' 
					println JWT_KEY_CRED_ID
					println HUB_ORG
					println SFDC_HOST
					println CONNECTED_APP_CONSUMER_KEY
                    sh "printenv"
					
					sh "ls -la"
					sh "cd force-app"
					sh "pwd"
				
					//if (isUnix()) {
					//	rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
					//}else{
					//	rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
					//}
					//if (rc != 0) { error 'hub org authorization failed' }
					//
					//		println rc
					//		
					//		// need to pull out assigned username
					//		if (isUnix()) {
					//			rmsg = sh returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
					//		}else{
					//		rmsg = bat returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
					//		}
					//		
					//printf rmsg
					//println('Hello from a Job DSL script!')
					//println(rmsg)
					
				}
				dir("force-app/main/default/classes/") {
					sh "pwd"
					sh "ls -la"
					
					stage("clean"){
						sh "pwd"
						sh "ls -la"
						sh 'sfdx --help'
						cleanWs()
					}
				}
			}
		}
    }
}
