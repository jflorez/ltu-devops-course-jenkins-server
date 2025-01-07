FROM jenkins/jenkins:jdk21

USER root

# Install necessary packages including Docker
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
    sudo \
    mariadb-client && \
    mkdir -p /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
    $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y docker-ce docker-compose-plugin

# Create docker group and add jenkins user to it
RUN groupadd -f docker && usermod -aG docker jenkins && \
    echo "jenkins ALL=(ALL) NOPASSWD: /usr/bin/chmod" >> /etc/sudoers && \
    echo "jenkins ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Install Node.js 22.x
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm@latest && \
    corepack enable

# Switch to jenkins user for plugin installation
USER jenkins

# Run initial setup
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=true

# Copy the plugins.txt file
COPY --chown=jenkins:jenkins plugins.txt /usr/share/jenkins/ref/plugins.txt

# Install plugins
RUN jenkins-plugin-cli -f /usr/share/jenkins/ref/plugins.txt

# Set custom port
ARG JENKINS_PORT=8088
ENV JENKINS_OPTS --httpPort=${JENKINS_PORT}

# Copy JCasC configuration
COPY --chown=jenkins:jenkins jenkins.yaml /var/jenkins_home/jenkins.yaml
ENV CASC_JENKINS_CONFIG=/var/jenkins_home/jenkins.yaml

# Setup entrypoint
USER root
COPY entrypoint.sh /usr/local/bin/jenkins-entrypoint.sh
RUN chmod +x /usr/local/bin/jenkins-entrypoint.sh

USER jenkins
ENTRYPOINT ["/usr/local/bin/jenkins-entrypoint.sh"]