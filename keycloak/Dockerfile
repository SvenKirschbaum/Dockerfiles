FROM quay.io/keycloak/keycloak:26.1.0@sha256:ac430f92e286fdcbef10bc8ea6e2b2472eae5838dc264221be338f6733453e70 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,persistent-user-sessions
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:26.1.0@sha256:ac430f92e286fdcbef10bc8ea6e2b2472eae5838dc264221be338f6733453e70
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
