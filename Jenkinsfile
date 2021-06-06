pipeline {
    agent any

    stages{
        stage("Develop preparation"){
            
            steps {
                
                     // GIT submodule recursive checkout
                checkout scm: [
                        $class: 'GitSCM',
                        branches: scm.branches,
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [[$class: 'SubmoduleOption',
                                      disableSubmodules: false,
                                      parentCredentials: false,
                                      recursiveSubmodules: true,
                                      reference: '',
                                      trackingSubmodules: false]],
                        submoduleCfg: [],
                        userRemoteConfigs: scm.userRemoteConfigs
                ]
				
            }
        }
    	stage("Test docker app"){
    		steps {
    			sh '''
					echo "******** Docker Test BUILD ******** "
					docker-compose up -d --build
				'''
    		}
    	}
		stage("Deploy preparation"){
			steps{
				sh '''
					echo "********** Docker PROD BUILD **********"
					docker build -t rodlati03/docker-app-prod .
				'''
			}
		}
		
		stage("Deploy docker app"){
			steps{
				sh '''
					docker run -p 5000:80 rodlati03/docker-app-prod npm run test -- --coverage
				'''
			}
		}
     }

}
