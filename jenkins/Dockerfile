FROM jenkins/jenkins:lts

USER root

# Install Docker CLI and Compose plugin (no daemon)
RUN apt-get update && \
    apt-get install -y ca-certificates curl gnupg rsync && \
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
    chmod a+r /etc/apt/keyrings/docker.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
    $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    apt-get update && \
    apt-get install -y docker-ce-cli docker-compose-plugin && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Create docker group with host GID and add jenkins user
RUN groupadd -g 989 docker && usermod -aG docker jenkins

# Create host-mapped workspace parent so Jenkins can write @tmp dirs
RUN mkdir -p /home/tgnazmi && chown jenkins:jenkins /home/tgnazmi

USER jenkins
