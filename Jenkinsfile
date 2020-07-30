properties([
  parameters([
    string(name: 'INSTANCE_NAME', defaultValue: 'af-dev-instance'),
  ])
])
def INSTANCE_IP=''
node {
  stage("Check AWS") {
  	withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-cli', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
  		INSTANCE_IP = sh script: """
	  			 docker run --rm \
					 -e AWS_ACCESS_KEY_ID=${AWS_ACCESS_KEY_ID} \
					 -e AWS_SECRET_ACCESS_KEY=${AWS_SECRET_ACCESS_KEY} \
					 -e AWS_DEFAULT_REGION="eu-central-1" \
					 amazon/aws-cli \
					 ec2 describe-instances --query "Reservations[*].Instances[*].PublicIpAddress" --filter "Name=tag:Name,Values=${INSTANCE_NAME}" --output=text
	  			""", returnStdout:true
	  	INSTANCE_IP = INSTANCE_IP.trim()
  	}
  }
  stage("Checkout") {
  	checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'github', url: 'git@github.com:Cristopherus/AwsTest.git']]])
  }
  // stage("Docker build"){

  // 	docker.withRegistry('https://396427124739.dkr.ecr.eu-central-1.amazonaws.com', "ecr:eu-central-1:aws-cli") {
  //       def profile_docker_image = docker.build("my_nginx")
  //       profile_docker_image.push()
  //   }
  // }
  stage("SSH") {
    withCredentials([string(credentialsId: 'sekret', variable: 'db_pass')]) {
    	sshagent (credentials: ['aws']) {
    		sh "echo 'STAGE SSH'"
    		sh "echo ${INSTANCE_IP}"
        sh "echo 'SEKRET"
        sh "echo ${db_pass}"
    		// sh "scp -o StrictHostKeyChecking=no docker-compose.yml ec2-user@${INSTANCE_IP}:/home/ec2-user/"
    		// sh "ssh ec2-user@${INSTANCE_IP} -o StrictHostKeyChecking=no 'sudo docker stack deploy -c docker-compose.yml --with-registry-auth nginx'"
    	}
    }
  }
}