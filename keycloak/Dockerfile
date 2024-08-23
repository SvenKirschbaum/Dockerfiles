FROM quay.io/keycloak/keycloak:25.0.4@sha256:bf788a3b7fd737143f98d4cb514cb9599c896acee01a26b2117a10bd99e23e11 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,admin2,persistent-user-sessions
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:25.0.4@sha256:bf788a3b7fd737143f98d4cb514cb9599c896acee01a26b2117a10bd99e23e11
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
