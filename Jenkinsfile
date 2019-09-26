node 
{
	stage('Choose artifacts    (Manual action needed)')
    {
        def artifact = input message: 'download artifacts', parameters: [[$class: 'ExtensibleChoiceParameterDefinition', choiceListProvider: [$class: 'NexusChoiceListProvider', artifactId: 'petclinic', classifier: '', credentialsId: 'nexus-id', groupId: 'com.cicd', packaging: 'war', repositoryId: 'petclinic', url: 'http://10.0.2.18:8081/nexus'], description: '', editable: false, name: 'artifacts']]
        println "${artifact}"
        sh "rm -rf /home/ec2-user/nexus_artifacts/petclinic.war"
        sh "rm -rf /home/ec2-user/nexus_artifacts/petclinic.zip"
        echo "${artifact}"
        sh "curl -L '${artifact}' -o /home/ec2-user/nexus_artifacts/petclinic.war"
        sh "cp /home/ec2-user/nexus_artifacts/petclinic.war /home/ec2-user/nexus_artifacts/petclinic.zip"
        sh "sudo scp -o StrictHostKeyChecking=no -i /home/ec2-user/Chandra_Oregon.pem /home/ec2-user/nexus_artifacts/petclinic.zip ec2-user@10.0.2.20:/tmp/petclinic.zip"
        sh "sleep 05"

    }
    
     stage("Deploy to Rundeck")
	{
	   step([$class: "RundeckNotifier", 
            includeRundeckLogs: true, 
            jobId: "fe8f8e40-dc53-4c61-bd7a-afb7014b727b", 
            rundeckInstance: "Rundeck", 
            shouldFailTheBuild: true, 
            shouldWaitForRundeckJob: true, 
            tailLog: true]) 

	}
    
}
    
