node {
    def server = Artifactory.server('sanoop.jfrog.io')
    def buildInfo = Artifactory.newBuildInfo()
    def rtMaven = Artifactory.newMavenBuild()
    
    
    stage ('Checkout & Build') {
        git url: 'https://github.com/Itrain-Breaking-bad/jfrog-maven.git'
    }
 
    stage ('Code Build') {
        rtMaven.tool = 'Maven3.6.1' // Tool name from Jenkins configuration
        rtMaven.run pom: 'pom.xml', goals: 'clean compile'
    }
    
    stage ('UnitTest') {
        rtMaven.tool = 'Maven3.6.1' // Tool name from Jenkins configuration
        rtMaven.run pom: 'pom.xml', goals: 'test'
    }
    
    stage('SonarQube Analysis') {
        //rtMaven.run pom: 'maven-example/pom.xml', goals: 'clean org.jacoco:jacoco-maven-plugin:prepare-agent package sonar:sonar -Dsonar.host.url=https://sonarcloud.io -Dsonar.organization=pattabhi -Dsonar.login=df5bb81bae9ba310d6a38135b957227ba6ecd32c '
     
         
      }
    
    stage ('Artifactory configuration') {
        // Obtain an Artifactory server instance, defined in Jenkins --> Manage..:
         
        rtMaven.tool = 'Maven3.6.1' // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'libs-release-local', snapshotRepo: 'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'libs-release', snapshotRepo: 'libs-snapshot', server: server
        rtMaven.deployer.deployArtifacts = false // Disable artifacts deployment during Maven run
     }
            
    stage ('Install') {
        rtMaven.run pom: 'pom.xml', goals: 'install', buildInfo: buildInfo
     }
 
    stage ('Deploy') {
        rtMaven.deployer.deployArtifacts buildInfo
    }
        
    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
    
    stage ('Deploy to Dev') {
        
    }
       
}
