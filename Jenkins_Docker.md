
## Getting started with Jenkins

https://benmatselby.dev/post/jenkins-basics/

### Getting started with Jenkins: Agents

https://benmatselby.dev/post/jenkins-basic-agent/

Create a network

```bash
JENKINS_NETWORK=jenkins
docker network create ${JENKINS_NETWORK}
```

Run Jenkins controller in a container named `jenkins-controller` on this network

```bash
JENKINS_CONTROLLER=jenkins-controller
# Replace with the desired version, see https://hub.docker.com/r/jenkins/jenkins/tags
JENKINS_VERSION=2.377-jdk11
docker run \
	-p 8080:8080 \
	-p 50000:50000 \
	--network ${JENKINS_NETWORK} \
	-d \
	-v jenkins_home:/var/jenkins_home \
	--name ${JENKINS_CONTROLLER} \
	jenkins/jenkins:${JENKINS_VERSION}
```

Add plugin `Docker pipeline`.

[Create a new node](

### Build Jenkins inbound agent image with Docker CLI inside

```bash
JENKINS_AGENT_DOCKER_IMAGE=jenkins-agent-docker
docker build -t ${JENKINS_AGENT_DOCKER_IMAGE} -f Dockerfile_Jenkins_agent_Docker_cli .
```

### Run Jenkins agent corresponding to node

Prepare the values as variables:

```bash
# Replace with the value of the secret
SECRET_FOR_AGENT=6733c03982a590432bbafb8ac8ea221041c8fd69b57f51655b1b511ff1933035
# Replace with the agent desired name
JENKINS_AGENT_NAME=smith
```

Run the agent in a container, in privileged allowing Docker in Docker (DIND):

```bash
docker run --rm \
 --privileged \
 -v /var/run/docker.sock:/var/run/docker.sock \
 -eJENKINS_SECRET=${SECRET_FOR_AGENT} \
 -eJENKINS_URL=http://${JENKINS_CONTROLLER}:8080 \
 -eJENKINS_AGENT_NAME=${JENKINS_AGENT_NAME} \
 --network ${JENKINS_NETWORK} \
 --init \
 -it \
 --name jenkins-agent_${JENKINS_AGENT_NAME} \
 -P ${JENKINS_AGENT_DOCKER_IMAGE}
```


