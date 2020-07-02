node {
   def mvnHome
   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git 'https://github.com/walidsaad/training-app-sii.git'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      mvnHome = tool 'maven3'
   }
   stage('SonarQube analysis') {
             // Run the maven build
      if (isUnix()) {
         sh "'${mvnHome}/bin/mvn' -B sonar:sonar"
      } else {
         bat(/"${mvnHome}\bin\mvn" -B sonar:sonar/)
      }
        }
   stage('Build') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" -DskipTests clean package'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
     stage('Test') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh '"$MVN_HOME/bin/mvn" test'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   
   stage('Results') {
      junit '**/target/surefire-reports/TEST-*.xml'
      archiveArtifacts 'target/*.jar'
   }
   
     stage('Execute JAR') {
      // Run the maven build
      withEnv(["MVN_HOME=$mvnHome"]) {
         if (isUnix()) {
            sh 'java -jar target/training-app-1.0-SNAPSHOT.jar'
         } else {
            bat(/"%MVN_HOME%\bin\mvn" -Dmaven.test.failure.ignore clean package/)
         }
      }
   }
   
   stage('Publish Artefact') {            
      if (isUnix())
      {       
      sh "'${mvnHome}/bin/mvn' -Dmaven.test.skip=true deploy -s /home/stagiaire/tools/apache-maven-3.6.3/conf/settings.xml"
      } 
      else 
      {        
      bat(/"${mvnHome}\bin\mvn" -Dmaven.test.skip=true deploy -s "C:\Program Files (x86)\apache-maven-3.5.3\conf\settings.xml"/)      
      }
   }
}
