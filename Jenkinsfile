pipeline {
  agent any
  stages {
  
   stage('BUILD') {
	environment {
        SECRET_FILE_ID = credentials('secret-file')
      }
     steps {
	   echo "####DISPLAYING SECRET_FILE_ID####"
	   echo "Global property file: ${SECRET_FILE_ID}"
	   
	   echo "#####Copying Global property file to src\\main\\resources#####"
	   bat "powershell Copy-Item ${SECRET_FILE_ID} -Destination src\\main\\resources"
	   
	   echo "#####Deleting Local Global property file present in src\\main\\resources#####"
	   bat "powershell Remove-Item src\\main\\resources\\application.properties"
	   
	   echo "#####Renaming Global property file in Jenkins to global-dev.properties#####"
	   bat "powershell Rename-Item src\\main\\resources\\secret_file_jenkins application.properties"
	   
	   echo "#####Creating Mule Application artifact#####"
	   bat 'mvn clean install'
	   }
   }
   
    stage('DEPLOY-DEV') {
      environment {
		CLOUDHUB_ENV = credentials('CLOUDHUB_ENV_SANDBOX')
		ANYPOINT_USERNAME_DEV = credentials('ANYPOINT_USERNAME_DEV')
		ANYPOINT_PASSWORD_DEV = credentials('ANYPOINT_PASSWORD_DEV')		
      }
      steps {
	   echo "DISPLAYING ALL ENVIRONMENT VARIABLES...."
	   echo "CLOUDHUB_ENV: ${CLOUDHUB_ENV}"
	   echo "ANYPOINT_USERNAME: ${ANYPOINT_USERNAME_DEV}"
	   echo "ANYPOINT_PASSWORD: ${ANYPOINT_PASSWORD_DEV}"
	   echo "#####Initiating Deployment to cloudhub#####"
	   bat "mvn package deploy -Pcloudhub -Denv=dev -DANYPOINT_USERNAME=${ANYPOINT_USERNAME_DEV} -DANYPOINT_PASSWORD=${ANYPOINT_PASSWORD_DEV} -DCLOUDHUB_ENV=${CLOUDHUB_ENV}"	  
	   echo "################APP SUCCESSFULLY DEPLOYED################"	   
      }
    }
  }
}