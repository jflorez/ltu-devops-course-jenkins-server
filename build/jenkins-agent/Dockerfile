FROM jenkins/agent:latest-jdk21

USER root

# Install necessary packages
RUN apt-get update && \
    apt-get install -y \
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
    sudo

# Install Node.js 22.x
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm@latest && \
    corepack enable

# Switch back to jenkins user
USER jenkins 