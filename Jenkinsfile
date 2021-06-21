pipeline {
    agent { label 'Linux' }
	environment {
		BUCKET_ROOT = 'elasticbeanstalk-us-east-2-806855832725'
		BUCKET_DOCKER_APP = 'elasticbeanstalk-us-east-2-806855832725/docker-app'
		docker_app_folder = ''
	}
    stages {
        stage("Git Check out"){
				
				steps {
					
					git credentialsId: 'Github', url: 'git@github.com:rodlati03/docker-react-app.git'
				}
			}
        stage('Creating archive Folder') {
            steps{
                sh '''
                    # remove zip file if it's found
                    if [ -f docker-app.zip ];then rm docker-app.zip;fi
                    echo "************ Archive the project into ZIP file ************"
                    #apt install zip -y
                    #zip -r docker-app.zip ${WORKSPACE}
                '''  
            }
        }
		
        
    }
    post {
        success {
            archiveArtifacts artifacts: '**/*', followSymlinks: false
         }
    }
}
