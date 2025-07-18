ARG BUILD_FROM
FROM ${BUILD_FROM}

# Install packages in a single layer to minimize image size
RUN apk add --no-cache \
    nodejs \
    npm \
    bash \
    curl \
    && npm install -g @anthropic-ai/claude-code@latest \
    && npm cache clean --force

# Install Home Assistant CLI
ARG BUILD_ARCH
ARG HA_CLI_VERSION="4.39.0"
RUN curl -L -s -o /usr/bin/ha \
    "https://github.com/home-assistant/cli/releases/download/${HA_CLI_VERSION}/ha_${BUILD_ARCH}" \
    && chmod a+x /usr/bin/ha

# Create directory for Claude configuration and set working directory
RUN mkdir -p /config/claude-config
WORKDIR /config

# Copy startup script and modular scripts
COPY run.sh /run.sh
COPY scripts/ /config/scripts/
RUN chmod +x /run.sh \
    && chmod +x /config/scripts/*.sh

# Use exec form for better signal handling
CMD ["/run.sh"]