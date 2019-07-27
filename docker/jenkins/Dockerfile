from jenkins/jenkins

# install plugins
RUN /usr/local/bin/install-plugins.sh \
  github \
  ssh-slaves \
  pipeline \
  azure credentials \
  kubernetes
  
USER jenkins
