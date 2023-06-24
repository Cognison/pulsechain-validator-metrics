# Use the official Promtail base image
FROM grafana/promtail:2.3.0

# Copy the Promtail configuration file into the container
COPY promtail-config.yaml /etc/promtail/promtail-config.yaml

# Set the command to run Promtail with the specified configuration
CMD ["-config.file=/etc/promtail/promtail-config.yaml"]