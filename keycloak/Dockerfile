FROM quay.io/keycloak/keycloak:26.4.0@sha256:5f3fb534cde6bf006d79f5912473e5d2c828c707cdfc52e16972803aca9d43dd as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,persistent-user-sessions
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:26.4.0@sha256:5f3fb534cde6bf006d79f5912473e5d2c828c707cdfc52e16972803aca9d43dd
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
