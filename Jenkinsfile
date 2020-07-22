node {

  // withCredentials([sshUserPrivateKey(credentialsId: 'jenkins-aws', keyFileVariable: 'jenkins-aws', passphraseVariable: '', usernameVariable: '')]) {
  //   sh "echo $jenkins-aws > key"
  //   sh "ssh ec2-user@3.123.35.186 -i key 'touch > ala.txt'"
  // }
  stage("Hello") {
  	sh "echo 'Hello World!!'"
  }
  stage("SSH") {
  	sshagent (credentials: ['aws']) {
  		sh "ssh ec2-user@35.159.22.97 -vvv 'touch ala.txt'"
  	}
  }
}