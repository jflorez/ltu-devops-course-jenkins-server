FROM jenkins/jenkins:jdk21

###################
# System Setup    #
###################
USER root

###################
# System Packages #
###################
RUN apt-get update && \
    apt-get install -y \
        apt-transport-https \
        ca-certificates \
        curl \
        gnupg \
        lsb-release \
        build-essential \
        python3 \
        make \
        g++ \
        sudo

###################
# Docker Setup    #
###################
RUN mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | \
        gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
        https://download.docker.com/linux/debian $(lsb_release -cs) stable" | \
        tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y docker-ce docker-compose-plugin

###################
# Permissions     #
###################
RUN groupadd -f docker && \
    usermod -aG docker jenkins && \
    echo "jenkins ALL=(ALL) NOPASSWD: /usr/bin/chmod" >> /etc/sudoers && \
    echo "jenkins ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

###################
# Jenkins Config  #
###################
USER jenkins

# Setup Jenkins environment
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=true
ENV CASC_JENKINS_CONFIG=/var/jenkins_home/jenkins.yaml

# Install Jenkins plugins
COPY --chown=jenkins:jenkins plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt

# Configure Jenkins port
ARG JENKINS_PORT=8088
ENV JENKINS_OPTS --httpPort=${JENKINS_PORT}

# Copy Jenkins configuration
COPY --chown=jenkins:jenkins jenkins.yaml /var/jenkins_home/jenkins.yaml

###################
# Entrypoint     #
###################
USER root
COPY entrypoint.sh /usr/local/bin/jenkins-entrypoint.sh
RUN chmod +x /usr/local/bin/jenkins-entrypoint.sh

USER jenkins
ENTRYPOINT ["/usr/local/bin/jenkins-entrypoint.sh"]