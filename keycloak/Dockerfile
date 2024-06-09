FROM quay.io/keycloak/keycloak:24.0.5@sha256:c916c668a5cd589948c3310ab31ffcfc5da55f0e546028f2f606419ce17c6ad8 as builder

ENV KC_HEALTH_ENABLED=false
ENV KC_FEATURES=token-exchange,admin2
ENV KC_DB=mariadb

ADD --chown=keycloak:keycloak https://github.com/dreezey/argon2-password-hash-provider/releases/download/2.0.1/argon2-password-hash-provider-2.0.1.jar /opt/keycloak/providers/argon2-password-hash-provider-2.0.1.jar
RUN /opt/keycloak/bin/kc.sh build

FROM quay.io/keycloak/keycloak:24.0.5@sha256:c916c668a5cd589948c3310ab31ffcfc5da55f0e546028f2f606419ce17c6ad8
COPY --from=builder /opt/keycloak/ /opt/keycloak/
WORKDIR /opt/keycloak
ENTRYPOINT ["/opt/keycloak/bin/kc.sh", "start", "--optimized"]
