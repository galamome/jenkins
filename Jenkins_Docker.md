
## Getting started with Jenkins

https://benmatselby.dev/post/jenkins-basics/

### Getting started with Jenkins: Agents

https://benmatselby.dev/post/jenkins-basic-agent/

Create a network

```bash
docker network create jenkins
```

Run Jenkins controller in a container named `jenkins-controller` on this network

```bash
docker run -p 8080:8080 \
	-p 50000:50000 \
	--network jenkins \
	-d \
	-v jenkins_home:/var/jenkins_home \
	--name jenkins-controller \
	jenkins/jenkins:lts
```

[Create a new node](

Prepare the values as variables:

```bash
# Replace with the value of the secret
SECRET_FOR_AGENT=6733c03982a590432bbafb8ac8ea221041c8fd69b57f51655b1b511ff1933035
# Replace with the desired version, see https://hub.docker.com/r/jenkins/inbound-agent/tags
JENKINS_INBOUND_AGENT_VERSION=4.10-3-alpine-jdk11
# Replace with the agent desired name
JENKINS_AGENT_NAME=smith
```

Run the agent in a container named `jenkins_agent` on this network :

```bash
docker run --rm \
 -eJENKINS_SECRET=${SECRET_FOR_AGENT} \
 -eJENKINS_URL=http://jenkins-controller:8080 \
 -eJENKINS_AGENT_NAME=${JENKINS_AGENT_NAME} \
 --network jenkins \
 --init \
 -it \
 --name jenkins-agent_${JENKINS_AGENT_NAME} \
 jenkins/inbound-agent:${JENKINS_INBOUND_AGENT_VERSION}
```


