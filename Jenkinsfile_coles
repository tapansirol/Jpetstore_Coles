node {
	def majorVersion="1.0.${BUILD_NUMBER}"
		currentBuild.displayName = majorVersion
	
		//currentBuild.displayName = "1.0.${BUILD_NUMBER}"
	def GIT_COMMIT
  stage ('Checkput Source Code'){
	  
      def scm = git 'https://github.com/tapansirol/Jpetstore_Coles'
	  GIT_COMMIT = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
	  echo "COMMITID ${GIT_COMMIT}"
	  //echo "BBBB ${scm}"
	  //GIT_COMMIT = scm.GIT_COMMIT
	   //echo "**** ${GIT_COMMIT}"
	  
  }
	
 
  stage ('Build') {
      withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
      sh 'mvn clean package'
	      echo "**** ${GIT_COMMIT}"
	//step($class: 'UploadBuild', tenantId: "5ade13625558f2c6688d15ce", revision: "${GIT_COMMIT}", appName: "JPetStore", requestor: "admin", id: "${newComponentVersionId}" )
	
	     
    }
  }
  

  stage ('Cucumber'){
	  withMaven(jdk: 'JDK_local', maven: 'MVN_Local') {
		  sh 'mvn test -Dtest=Runner'    
	  }
		//cucumber buildStatus: "Success",
		//	fileIncludePattern: "**/cucumber.json",
		//	jsonReportDirectory: 'target'

  }
  
  	echo("************************** Test Result Upload Started to Velocity****************************")
                          try{
                          step([$class: 'UploadJUnitTestResult',
                              properties: [
                          // Need to change the path of the test result xml result required.               
                                  filePath: "target/surefire-reports/TEST-org.mybatis.jpetstore.service.OrderServiceTest.xml",
                                  tenant_id: "5ade13625558f2c6688d15ce",
                                  appName: "JPetStore-velocity",
                                  //appExtId: "4b006cdb-0e50-43f2-ac87-a7586a65389e",
  				appExtId: "698c01d4-0eed-484f-8c2b-33395f8dfc46",
  				//appId: "acdfae67-616f-43e5-8872-2cfa3aa583de",    
                                  name: "Executed in JUnit - 1.0.${BUILD_NUMBER}",
                                  testSetName: "Junit Test Run from Jenkins"]
                             
                          ])}catch(e){
                          throw e
                          }
                         
            echo("************************** Test Result Uploaded Successful to Velocity****************************")

	stage('SonarQube Analysis'){
		def mvnHome = tool name : 'MVN_Local', type:'maven'
		withSonarQubeEnv('sonar-server'){
			 "SONAR_USER_HOME=/opt/bitnami/jenkins/.sonar ${mvnHome}/bin/mvn sonar:sonar"
			sh  "${mvnHome}/bin/mvn sonar:sonar -Dsonar.projectKey=jpetstore -Dsonar.projectName=JPetStore"
		}
	}
	
	
stage ("Appscan"){
	//sleep 40
	//appscan application: '84963f4f-0cf4-4262-9afe-3bd7c0ec3942', credentials: 'Credential for ASOC', failBuild: true, failureConditions: [failure_condition(failureType: 'high', threshold: 100)], name: '84963f4f-0cf4-4262-9afe-3bd7c0ec39429048', scanner: static_analyzer(hasOptions: false, target: '/var/jenkins_home/jobs/jpetstore'), type: 'Static Analyzer', wait: true
 }
  stage('Publish Artificats to UCD'){
	  
   step([$class: 'UCDeployPublisher',
        siteName: 'ucd-server',
        component: [
            $class: 'com.urbancode.jenkins.plugins.ucdeploy.VersionHelper$VersionBlock',
            componentName: 'JPetStorevelocityComponent',
            createComponent: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.ComponentHelper$CreateComponentBlock',
                componentTemplate: '',
                componentApplication: 'JPetStore-velocity'
            ],
            delivery: [
                $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeliveryHelper$Push',
                pushVersion: '1.0.${BUILD_NUMBER}',
                //baseDir: '/var/jenkins_home/workspace/JPetStore/target',
		    baseDir: '/var/jenkins_home/workspace/${JOB_NAME}/target',
                fileIncludePatterns: '*.war',
                fileExcludePatterns: '',
               // pushProperties: 'jenkins.server=Jenkins-app\njenkins.reviewed=false',
                pushDescription: 'Pushed from Jenkins'
            ]
        ]
    ])
	  
		//sh 'env > env.txt'
	//	readFile('env.txt').split("\r?\n").each {
	//	println it
	//	}
	echo "(*****)"
	//  echo "${UUID}"
	
	//  echo "Demo1234 ${JpetComponent_VersionId}"
	//  def newComponentVersionId = "${JpetComponent_VersionId}"
	//  step($class: 'UploadBuild', tenantId: "5ade13625558f2c6688d15ce", revision: "${GIT_COMMIT}", appName: "JPetStore", requestor: "admin", id: "${newComponentVersionId}" )
	//  echo "Demo123 ${newComponentVersionId}"
	//sleep 25
	//  step([$class: 'UCDeployPublisher',
	//	deploy: [ createSnapshot: [deployWithSnapshot: true, 
	//		 snapshotName: "1.${BUILD_NUMBER}"],
	//		 deployApp: 'JPetStore', 
	//		 deployDesc: 'Requested from Jenkins', 
	//		 deployEnv: 'JPetStore_Dev', 
	//		 deployOnlyChanged: false, 
	//		 deployProc: 'Deploy-JPetStore', 
	//		 deployReqProps: '', 
	//		 deployVersions: "JpetComponent:1.${BUILD_NUMBER}"], 
	//	siteName: 'ucd-server'])
 
 echo "Demo1234 ${JPetStorevelocityComponent_VersionId}"
 	  def newComponentVersionId = "${JPetStorevelocityComponent_VersionId}"
 	  echo "git commit ${GIT_COMMIT}"
 	  //step($class: 'UploadBuild', tenantId: "5ade13625558f2c6688d15ce", revision: "${GIT_COMMIT}", appName: "Altoro", requestor: "admin", id: "${newComponentVersionId}" )
  step($class: 'UploadBuild', 
        tenantId: "5ade13625558f2c6688d15ce", 
        revision: "${GIT_COMMIT}", 
        appName: "JPetStore-velocity", 
        requestor: "admin", 
        id: "${newComponentVersionId}", 
        versionName: "1.0.${BUILD_NUMBER}"
       )
      
 	//echo "Demo123 ${newComponentVersionId}"
 	//sleep 25
 	  step([$class: 'UCDeployPublisher',
 		deploy: [ createSnapshot: [deployWithSnapshot: true, 
 			 snapshotName: "1.0.${BUILD_NUMBER}"],
 			 deployApp: 'JPetStore-velocity', 
 			 deployDesc: 'Requested from Jenkins', 
 			 deployEnv: 'JPetStore-velocity_Dev', 
 			 deployOnlyChanged: false, 
 			 deployProc: 'Deploy-JPetStore-velocity', 
 			 deployReqProps: '', 
 			 deployVersions: "JPetStorevelocityComponent:1.0.${BUILD_NUMBER}"], 
 		siteName: 'ucd-server'])
 }
 
stage ('HCL One Test') {
	sleep 25
	// echo 'Executing HCL One test ... '
	//sh '/var/jenkins_home/onetest/hcl-onetest-command.sh'
 }

}
