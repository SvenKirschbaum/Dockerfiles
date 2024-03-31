FROM quay.io/keycloak/keycloak:24.0.2@sha256:b8a3f00fc433f2999bc304b73df331e2005037e8f5673f644f9c0eacd5fbe048 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,admin2
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:24.0.2@sha256:b8a3f00fc433f2999bc304b73df331e2005037e8f5673f644f9c0eacd5fbe048
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
