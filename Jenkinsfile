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
			// :latestsh "docker build -t ${ImageName}:${imageTag} ."
			sh "docker build --rm -t ${ImageName}:latest ."
			sh "docker push ${ImageName}:latest"
		}

	}
	
	withKubeConfig(caCertificate: '', clusterName: 'minikube', contextName: 'minikube', credentialsId: 'Minikube', serverUrl: 'https://192.168.99.100:8443') {
		stage('Deploy FORCARE APP on K8s'){
			// Setting up Forcare App on Kubernetes
			// clean up
			sh "kubectl delete -f forcareapp-pod.yaml"
			// sh "kubectl delete svc forcareapp-deployment"
			// create app
			sh "kubectl create -f forcareapp-pod.yaml"
			// for testing only
			// sh "kubectl create deployment hello-node --image=gcr.io/hello-minikube-zero-install/hello-node"
			sh "kubectl get pods"
			// sh "kubectl expose deployment forcareapp-deployment --type=NodePort"
			// todo using Ansible
			// sh "cp -r /${WORKSPACE}/ansible/app-deploy /var/lib/jenkins/"
			// todo using Helm
		}
		// stage('Deploy ELK APP on K8s'){
			// Setting up Elasticsearch cluster on Kubernetes
			// clean up
			// sh "kubectl delete elasticsearch"
			// sh "kubectl delete svc elasticsearch"
			// create app
			// sh "kubectl run elasticsearch --image=docker.elastic.co/elasticsearch/elasticsearch:6.5.4  --env="discovery.type=single-node" --port=9200"
			// sh "kubectl get pods"
			// sh "kubectl expose deployment elasticsearch --type=NodePort"
			// Setup Kibana
			// sh "kubectl run kibana --image=docker.elastic.co/kibana/kibana:6.5.4 --env="ELASTICSEARCH_URL=http://elasticsearch:9200" --env="XPACK_SECURITY_ENABLED=true" --port=5601"
			// sh "kubectl expose deployment kibana --type=NodePort"
		// }
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