#!groovy
 
import groovy.json.JsonSlurperClassic

node {	
	println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG
    println SFDC_HOST
    println CONNECTED_APP_CONSUMER_KEY
	
	
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