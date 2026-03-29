FROM alpine:latest

# 1. Install CLI, plugins, curl, pass, and gnupg 
# (--no-cache handles the index update automatically)
RUN apk add --no-cache \
    openssh-client \
    docker-cli \
    docker-cli-compose \
    docker-cli-buildx \
    pass \
    gnupg \
    curl

# 2. Dynamically fetch the correct credential-helper binary (Bumped to v0.9.5)
RUN ARCH=$(uname -m) && \
    case "$ARCH" in \
        x86_64) DOCKER_ARCH="amd64" ;; \
        aarch64) DOCKER_ARCH="arm64" ;; \
        *) echo "Unsupported architecture" && exit 1 ;; \
    esac && \
    curl -fsSL "https://github.com/docker/docker-credential-helpers/releases/download/v0.9.5/docker-credential-pass-v0.9.5.linux-${DOCKER_ARCH}" -o /usr/local/bin/docker-credential-pass && \
    chmod +x /usr/local/bin/docker-credential-pass

# 3. Setup the non-root user
RUN adduser -D -h /home/dockeruser dockeruser
USER dockeruser
WORKDIR /home/dockeruser

# 4. Configure GPG for headless use BEFORE generating the key
RUN mkdir -p ~/.gnupg && \
    chmod 700 ~/.gnupg && \
    echo "pinentry-mode loopback" > ~/.gnupg/gpg.conf && \
    echo "allow-loopback-pinentry" > ~/.gnupg/gpg-agent.conf

# 5. Generate GPG key & Init Pass
RUN gpg --batch --passphrase '' --quick-generate-key "dockeruser" default default && \
    pass init "dockeruser"

# 6. Tell Docker to use the helper
RUN mkdir -p ~/.docker && \
    echo '{"credsStore": "pass"}' > ~/.docker/config.json

# 7. Ensure GPG_TTY is set whenever a shell starts
RUN echo "export GPG_TTY=\$(tty)" >> ~/.shrc
ENV ENV="/home/dockeruser/.shrc"

CMD ["/bin/sh"]
