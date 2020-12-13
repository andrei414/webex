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
			stage("Inject Env Variables") {
				env.HUB_ORG_DH="conttest414-aad2@force.com"
				env.SFDC_HOST_DH="https://login.salesforce.com"
				env.ONNECTED_APP_CONSUMER_KEY_DH="3MVG9SOw8KERNN0.kF.gZhK.3VoVL65c2VncoLTiigo1vv50w4m8GQiUw5VoIURBHzBvmt9W_u_NyvEM7ES78"
				env.JWT_CRED_ID_DH="3e95ac4d-61d5-4b8c-937c-8050ccd12b89"
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
				
					if (isUnix()) {
						rc = sh returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
					}else{
						rc = bat returnStatus: true, script: "sfdx force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY} --username ${HUB_ORG} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
					}
					if (rc != 0) { error 'hub org authorization failed' }
		
							println rc
							
							// need to pull out assigned username
							if (isUnix()) {
								rmsg = sh returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
							}else{
							rmsg = bat returnStdout: true, script: "sfdx force:mdapi:deploy -d manifest/. -u ${HUB_ORG}"
							}
							
					//printf rmsg
					//println('Hello from a Job DSL script!')
					//println(rmsg)
					
				}
				stage("clean"){
					cleanWs()
				}
			}
		}
    }
}