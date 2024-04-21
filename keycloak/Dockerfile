FROM quay.io/keycloak/keycloak:24.0.3@sha256:0d71412ed56b5f66f7e922efc0c3ff73b3f54ba2fe84fedb5e281a1f90b95bd6 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,admin2
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:24.0.3@sha256:0d71412ed56b5f66f7e922efc0c3ff73b3f54ba2fe84fedb5e281a1f90b95bd6
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
