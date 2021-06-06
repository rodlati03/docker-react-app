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
					echo "************* Check if docker-compose test is UP or DOWN *********** "
					IS_DOCKER_COMPOSE_UP=$(docker-compose ps|grep -i up|wc -l)
					if [ $IS_DOCKER_COMPOSE_UP -gt 0 ]
					then 
						echo "------ Docker compose is UP, trying to get it down --------- "
						docker-compose down
					fi					
						echo "******** Docker Test BUILD ******** "						
						docker-compose up -d --build
				'''
    		}
    	}
		stage("Deploy preparation"){
			steps{
				sh '''
					#IS_DOCKER_IMAGE_EXISTS=$(docker images | grep -i docker-app-prod | wc -l)
					#if [ $IS_DOCKER_IMAGE_EXISTS -eq 0 ]
					#then 
							echo "********** Docker PROD BUILD **********"
							docker build -t rodlati03/docker-app-prod .
					#fi				
				'''
			}
		}
		
		stage("Deploy docker app"){
			steps{
				sh '''
					docker --name docker-prod-container run -p 5000:80 rodlati03/docker-app-prod
				'''
			}
		}
     }

}
