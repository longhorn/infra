FROM jenkins/inbound-agent:latest-jdk11

ENV GIT_VERSION 2.33.0

USER root

RUN apt-get update && \
    apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    gnupg2 \
    software-properties-common \
    python3 \
    python3-pip \
    make \
    libssl-dev \
    libghc-zlib-dev \
    libcurl4-gnutls-dev \
    libexpat1-dev \
    gettext \
    unzip \
    wget

RUN cd /tmp && \
    wget "https://github.com/git/git/archive/v${GIT_VERSION}.zip" -O git.zip && \
    unzip git.zip && \
    cd git-* && \
    make prefix=/usr/local all && \
    make prefix=/usr/local install && \
    rm -rf /tmp/git-*

#RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - && \
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
	#add-apt-repository -y \
    #"deb [arch=amd64] https://download.docker.com/linux/debian \
    #$(lsb_release -cs) \
    #stable" && \
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
    $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y \
    docker-ce \
    docker-ce-cli \
    containerd.io && \
    usermod -aG docker jenkins && \
    newgrp docker

RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl && \
    chmod +x kubectl && \
    mv kubectl /usr/bin/kubectl

RUN pip3 install jenkins-job-builder

USER jenkins
