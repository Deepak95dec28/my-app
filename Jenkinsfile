node{
   stage('SCM Checkout'){
     git 'https://github.com/Deepak95dec28/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/firstpro.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
             }
   stage('Build Docker Imager'){
   sh 'docker build -t deepak95dec28/newweb:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPassword', variable: 'dockerPassword')]) {
   sh "docker login -u deepak95dec28 -p ${dockerPassword}"
    }
   sh 'docker push deepak95dec28/newweb:0.0.2'
   }
   //stage('Nexus Image Push'){
   //sh "docker login -u admin -p admin123 70.70.70.177:5000"
  // sh "docker tag deepak95dec28/newweb:0.0.2 70.70.70.177:5000/deepak:1.0.0"
   //sh 'docker push 70.70.70.177:5000/deepak:1.0.0'
   stage('splunk')
   //}
      stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8010:8080 --name tomcattest deepak95dec28/newweb:0.0.2' 
   }
}
}
