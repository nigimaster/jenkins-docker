## Jenkins-Docker 
### Description of the application
Jenkins-Docker container for local development
### Local environment setup
These are one-time activities.
1. `docker`, `docker-compose` and `git` are installed on the local workstation.
1. Clone this repo to the workstation and change directory into it.
```
   git https://github.com/nigimaster/jenkins-docker.git
   cd jenkins-docker
```
1. Build the docker image you need to run the `Forcare App Docker`. This is a one-time activity.
```
docker image build --rm -t jenkins-docker/jenkins:local .
```

### Running Jenins-Docker container locally
You want to develop and test the application locally before deploying to the target environments. This section explains how to do this.

1. Run the application
```
docker-compose up -d
```
1. The application can now be accessed on the local browser via the url 
```
http://localhost:8090
```
1. Once you are done, you may shutdown the local development environment.
```
docker-compose down
```
### Extra's
1. Log into the shell of the running container as root
```
docker exec -it -u root jenkins bash
```