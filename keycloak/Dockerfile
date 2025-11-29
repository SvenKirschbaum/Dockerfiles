FROM quay.io/keycloak/keycloak:26.4.6@sha256:d0d4037f17521a7f06137afd5a0eecb1f977f4ade773ae7755f1ee82cad8a576 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,persistent-user-sessions
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:26.4.6@sha256:d0d4037f17521a7f06137afd5a0eecb1f977f4ade773ae7755f1ee82cad8a576
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
