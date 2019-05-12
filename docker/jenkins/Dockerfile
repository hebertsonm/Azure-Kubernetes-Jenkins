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
  
USER jenkins
