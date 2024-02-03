FROM jetbrains/teamcity-agent:2023.11.2-linux-sudo
USER root
# Update docker from here, because the currently provided version (19) dosnt work with debian bullseye cgroups2
RUN curl -sL https://apt.corretto.aws/corretto.key | apt-key add - && \
    add-apt-repository --yes 'deb https://apt.corretto.aws stable main' && \
    curl -sL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | apt-key add - && \
    add-apt-repository --yes 'deb https://deb.nodesource.com/node_20.x nodistro main' && \
    add-apt-repository --yes ppa:longsleep/golang-backports && \
    apt-get install -y golang-go nodejs zip java-21-amazon-corretto-jdk docker-ce=5:24.\* docker-ce-cli=5:24.\* containerd.io make
USER buildagent
ENV JAVA_HOME=/usr/lib/jvm/java-21-amazon-corretto
