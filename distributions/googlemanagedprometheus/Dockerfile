FROM --platform=linux/amd64 alpine:3.13 as certs
RUN apk --update add ca-certificates

FROM --platform=linux/amd64 alpine:3.13 AS googlemanagedprometheus
COPY googlemanagedprometheus /googlemanagedprometheus
# Note that this shouldn't be necessary, but in some cases the file seems to be
# copied with the execute bit lost (see #1317)
RUN chmod 755 /googlemanagedprometheus

FROM scratch

ARG USER_UID=10001
USER ${USER_UID}

COPY --from=certs /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=googlemanagedprometheus /googlemanagedprometheus /
COPY configs/googlemanagedprometheus.yaml /etc/googlemanagedprometheus/config.yaml
ENTRYPOINT ["/googlemanagedprometheus"]
CMD ["--config", "/etc/googlemanagedprometheus/config.yaml"]
EXPOSE 4317 55678 55679
