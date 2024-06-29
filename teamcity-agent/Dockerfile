FROM jetbrains/teamcity-agent:2024.03.3-linux-sudo@sha256:b998583275a197f2c42b159963fb9696192337590b424499f33a0d22fe49e772
USER root
RUN curl -sL https://apt.corretto.aws/corretto.key | apt-key add - && \
    add-apt-repository --yes 'deb https://apt.corretto.aws stable main' && \
    curl -sL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | apt-key add - && \
    add-apt-repository --yes 'deb https://deb.nodesource.com/node_20.x nodistro main' && \
    add-apt-repository --yes ppa:longsleep/golang-backports && \
    apt-get install -y golang-go nodejs zip java-21-amazon-corretto-jdk make
USER buildagent
ENV JAVA_HOME=/usr/lib/jvm/java-21-amazon-corretto
