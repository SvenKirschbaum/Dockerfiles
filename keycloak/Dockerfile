FROM quay.io/keycloak/keycloak:25.0.6@sha256:82c5b7a110456dbd42b86ea572e728878549954cc8bd03cd65410d75328095d2 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,admin2,persistent-user-sessions
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:25.0.6@sha256:82c5b7a110456dbd42b86ea572e728878549954cc8bd03cd65410d75328095d2
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
