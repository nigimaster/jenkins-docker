#!groovy

node {
	currentBuild.result = "SUCCESS"
	def Namespace = "nigi"
	def ImageName = "nigi/forcareapp"
	def Creds = "DockerHub"

try {
	stage('Checkout'){
		git(
			url: 'https://github.com/nigimaster/demo.git',
			credentialsId: 'GitHub',
			branch: "master"
		)
		sh "git rev-parse --short HEAD > .git/commit-id"
		imageTag= readFile('.git/commit-id').trim()
	}

	// stage('RUN Unit Tests'){
		// sh ""
	// }

	stage('Docker Build, Push'){
		withDockerRegistry([credentialsId: "${Creds}", url: 'https://index.docker.io/v1/']) {
			sh "docker build -t ${ImageName}:${imageTag} ."
			sh "docker push ${ImageName}"
		}

	}
	
	withKubeConfig(caCertificate: '', clusterName: 'minikube', contextName: 'minikube', credentialsId: 'Minikube', serverUrl: 'https://192.168.99.100:8443') {
		stage('Deploy on K8s'){
			sh "kubectl create -f forcareapp-pod.yaml"
			// sh "kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node"
			sh "kubectl get pods"
			// sh "cp -r /${WORKSPACE}/ansible/app-deploy /var/lib/jenkins/"
			// sh "ansible-playbook ${WORKSPACE}/ansible/app-deploy/deploy.yml  --user=jenkins --extra-vars ImageName=${ImageName} --extra-vars imageTag=${imageTag} --extra-vars Namespace=${Namespace}"
		}
	}
}

catch (err) {
		currentBuild.result = "FAILURE"
		// mail body: "project build error is here: ${env.BUILD_URL}" ,
		// from: 'xxxx@yyyy.com',
		// replyTo: 'yyyy@yyyy.com',
		// subject: 'project build failed',
		// to: 'zzzz@yyyyy.com'
	throw err
	}
}