FROM jetbrains/teamcity-agent:2025.07.2-linux-sudo@sha256:a4477294e9b6b28f8d01caa1b45a65bcdf6994c5dec860d54bc7490e7356f6c9
USER root
RUN curl -sL https://apt.corretto.aws/corretto.key | apt-key add - && \
    add-apt-repository --yes 'deb https://apt.corretto.aws stable main' && \
    curl -sL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | apt-key add - && \
    add-apt-repository --yes 'deb https://deb.nodesource.com/node_24.x nodistro main' && \
    add-apt-repository --yes ppa:longsleep/golang-backports && \
    apt-get install -y golang-go nodejs zip java-25-amazon-corretto-jdk make && \
    echo '{ "mtu": 1400 }' > /etc/docker/daemon.json
USER buildagent
ENV JAVA_HOME=/usr/lib/jvm/java-25-amazon-corretto


RUN <<'eot' bash
  set -eux

  wait_for_upgrade() {
    local attempt=1
    local max_attempts=30
    local agent_log="/opt/buildagent/logs/teamcity-agent.log"

    echo $agent_log

    while [[ $attempt -le $max_attempts ]]; do
        if [[ -f "$agent_log" ]]; then
          if grep -q "Exit for upgrade" $agent_log || grep -q "The upgrade is complete" $agent_log; then
            echo "Agent upgrade complete"
            sleep 30
            return 0
          fi
        fi
        echo "Agent upgrade not yet complete"
        ((attempt++))
        sleep 15
      done
    echo "Agent upgrade did not complete after ${max_attempts} attempts"
    return 1
  }

  export SERVER_URL="https://dev.elite12.de/teamcity/"

  /run-agent.sh &
  PID=$!
  wait_for_upgrade
  kill $PID
  wait $PID || true
  rm -rf /opt/buildagent/logs/* /opt/buildagent/conf/buildAgent.properties
eot
