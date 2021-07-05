FROM evdokimovs/foobar-test:dev as base

FROM ubuntu as runtime

COPY --from=base /medea /

COPY config.toml /
COPY spec.yml /specs/

ENV MEDEA_CONF=/config.toml

CMD 'MEDEA_SERVER__CLIENT__HTTP__BIND_IP=$PORT /medea'
