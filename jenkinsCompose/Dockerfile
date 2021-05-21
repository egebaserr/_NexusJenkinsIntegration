FROM jenkins/jenkins:lts


USER root

# Install Docker from official repo
RUN apt-get update -qq && \
     apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
    apt-key fingerprint 0EBFCD88 && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" && \
    apt-get update -qq && \
    apt-get install -qqy docker-ce && \
    usermod -aG docker jenkins && \
    chown -R jenkins:jenkins $JENKINS_HOME/

# Install docker-compose from offical repo
RUN curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && \
	chmod 777 /usr/local/bin/docker-compose /


USER jenkins

VOLUME ['JENKINS_HOME', "/var/run/docker.sock"]

#skipping installation
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false
#installing config file
ENV CASC_JENKINS_CONFIG jenkins_data/casc.yaml
#installing plugins
COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt
#copying config file
COPY casc.yaml jenkins_data/casc.yaml
