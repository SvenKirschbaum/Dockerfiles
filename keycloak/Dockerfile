FROM quay.io/keycloak/keycloak:26.0.1@sha256:2f8c9679b3e3e9e9d253a62834ae63fb0c0dae55d2b2e4133b6d3f07762f2257 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,persistent-user-sessions
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:26.0.1@sha256:2f8c9679b3e3e9e9d253a62834ae63fb0c0dae55d2b2e4133b6d3f07762f2257
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
