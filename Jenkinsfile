node{

echo "job Name is:  ${env.JOB_NAME}"
echo "Node Name is:  ${env.NODE_NAME}"

def mavenHome = tool name: 'maven3.8.4'

//Get the code from GitHub Repo
stage('Checkoutcode'){
git branch: 'development', credentialsId: '681b12c5-1c0b-4770-8517-53adafc02add', url: 'https://github.com/shivaraj-devops/maven-web-application.git'
}
//Do the build by using Maven Build tool
stage('Build'){
sh "${mavenHome}/bin/mvn clean package"
}

//Execute the SonarQube Report
stage('ExecuteSonarQubeReport'){
sh "${mavenHome}/bin/mvn sonar:sonar"
}

//Upload Artifacts into Artifactory Repo
stage('UploadArtifactsintoNexus'){
sh "${mavenHome}/bin/mvn deploy"
}

//Deploy Application into Tomacat Server
stage('DeployApplicationIntoTomcatServer'){ 
sshagent(['2b2729bf-e66a-4f87-8c33-cf356e528014']) {
    sh "scp -o StrictHostkeyChecking=no target/maven-web-application.war ec2-user@43.204.98.235:/opt/apache-tomcat-9.0.62/webapps/"

}

}

}
