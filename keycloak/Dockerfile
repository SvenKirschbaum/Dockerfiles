FROM quay.io/keycloak/keycloak:26.3.0@sha256:08a31919cfcd814bf1b465142b1a716c4d1a8830f772bb5c9dffcbd96de3fba6 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,persistent-user-sessions
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:26.3.0@sha256:08a31919cfcd814bf1b465142b1a716c4d1a8830f772bb5c9dffcbd96de3fba6
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
