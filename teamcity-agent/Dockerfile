FROM jetbrains/teamcity-agent:2024.07-linux-sudo@sha256:a9c128b321b2a32a989eb5ce14764626365e3f3ae2a214ea0b60b55b84e97175
USER root
RUN curl -sL https://apt.corretto.aws/corretto.key | apt-key add - && \
    add-apt-repository --yes 'deb https://apt.corretto.aws stable main' && \
    curl -sL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | apt-key add - && \
    add-apt-repository --yes 'deb https://deb.nodesource.com/node_20.x nodistro main' && \
    add-apt-repository --yes ppa:longsleep/golang-backports && \
    apt-get install -y golang-go nodejs zip java-21-amazon-corretto-jdk make
USER buildagent
ENV JAVA_HOME=/usr/lib/jvm/java-21-amazon-corretto
