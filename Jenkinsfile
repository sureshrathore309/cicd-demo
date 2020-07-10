pipeline {
  agent any
  stages {
  
   stage('BUILD') {	   
	     steps {
	   echo "#####Creating Mule Application artifact#####"
	   bat 'mvn clean install'
   }
   }
  }
  }