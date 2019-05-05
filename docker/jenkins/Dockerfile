from jenkins/jenkins

# Distributed Builds plugins
RUN /usr/local/bin/install-plugins.sh ssh-slaves

# install plugins
RUN /usr/local/bin/install-plugins.sh \
  cloudbees-folder \
  email-ext \
  github \
  pipeline-build-step \
  pipeline-github-lib \
  pipeline-graph-analysis \
  pipeline-input-step \
  pipeline-milestone-step \
  pipeline-model-api \
  pipeline-model-declarative-agent \
  pipeline-model-definition \
  pipeline-model-extensions \
  pipeline-rest-api \
  pipeline-stage-step \
  pipeline-stage-tags-metadata \
  pipeline-stage-view \
  kubernetes
  
# Install Docker from official repo
USER root
RUN apt-get update -qq && \
    apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
    apt-key fingerprint 0EBFCD88 && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" && \
    apt-get update -qq && \
    apt-get install -qqy docker-ce && \
    usermod -aG docker jenkins && \
    chown -R jenkins:jenkins $JENKINS_HOME/
RUN usermod -aG docker jenkins
RUN service docker start
    
# Install maven
RUN apt-get update && apt-get install -y maven

USER jenkins
