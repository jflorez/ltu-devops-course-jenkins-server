FROM jenkins/agent:jdk21

###################
# System Setup    #
###################
USER root

###################
# System Packages #
###################
RUN apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    build-essential \
    python3 \
    python3-pip \
    make \
    g++ \
    sudo \
    mariadb-client

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
# Node.js Setup   #
###################
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm@latest && \
    corepack enable

###################
# Final Setup     #
###################
USER jenkins 