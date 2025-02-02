FROM rabbitmq:4.0-management
# FROM rabbitmq:3.12-management

# Install necessary tools
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    && rm -rf /var/lib/apt/lists/*

# Enable RabbitMQ plugins
RUN rabbitmq-plugins enable --offline \
    rabbitmq_management \
    rabbitmq_prometheus \
    rabbitmq_consistent_hash_exchange

# Set ulimits in container
RUN ulimit -n 65536

# Copy configuration files
COPY rabbitmq.conf /etc/rabbitmq/rabbitmq.conf
COPY definitions.json /etc/rabbitmq/definitions.json

# Create directories for volumes
RUN mkdir -p /var/lib/rabbitmq /var/log/rabbitmq && \
    chown -R rabbitmq:rabbitmq /var/lib/rabbitmq /var/log/rabbitmq

# Expose ports
EXPOSE 5672 15672 15692 4369 25672

# Default healthcheck
HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD rabbitmq-diagnostics check_port_connectivity

# Set environment variables
ENV RABBITMQ_SERVER_ADDITIONAL_ERL_ARGS="-rabbit disk_free_limit 2147483648"
