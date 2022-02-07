FROM alpine:3.13 as certs
RUN apk --update add ca-certificates


# Penser a faire export CGO_ENABLED=0 avant de lancer le build par "builder --config=config.yaml"
FROM alpine:3.13 AS otelcol
COPY otelcol-sfr /otelcolsfr
RUN chmod 755 /otelcolsfr

FROM scratch

ARG USER_UID=10001
USER ${USER_UID}

COPY --from=certs /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=otelcol /otelcolsfr /
COPY otelcol.yaml /etc/otelcol/config.yaml
ENTRYPOINT ["/otelcolsfr"]
CMD ["--config", "/etc/otelcol/config.yaml"]
EXPOSE 4317 55678 55679