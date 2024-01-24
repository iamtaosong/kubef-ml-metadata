FROM  gcr.io/tfx-oss-public/ml_metadata_store_server:1.8.0 as builder

FROM ubuntu:20.04

COPY --from=builder /bin/metadata_store_server /bin/metadata_store_server
COPY --from=builder /mlmd-src/third_party /mlmd-src/third_party  

ENV GRPC_PORT "8080"
ENV METADATA_STORE_SERVER_CONFIG_FILE ""

# Introduces tzdata package here to avoid LoadTimeZone check failed error in the metadata store server.
RUN export DEBIAN_FRONTEND=noninteractive && \
  apt-get update -y --option Acquire::Retries=3 && \
  apt-get install --no-install-recommends -y -q --option Acquire::Retries=3 \
  tzdata

ENTRYPOINT \
  "/bin/metadata_store_server" \
  "--grpc_port=${GRPC_PORT}" \
  "--metadata_store_server_config_file=${METADATA_STORE_SERVER_CONFIG_FILE}"
