# Use the official Loki base image
FROM grafana/loki:2.3.0

# Copy the Loki configuration file into the container
COPY loki-config.yaml /etc/loki/loki-config.yaml

# Set the command to run Loki with the specified configuration
CMD ["-config.file=/etc/loki/loki-config.yaml"]
