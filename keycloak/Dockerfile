FROM quay.io/keycloak/keycloak:26.2.2@sha256:df40860c716d0c2f941967e8ac3ca5e74094c09988298b2eb0ae9335ed34f8f6 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,persistent-user-sessions
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:26.2.2@sha256:df40860c716d0c2f941967e8ac3ca5e74094c09988298b2eb0ae9335ed34f8f6
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
