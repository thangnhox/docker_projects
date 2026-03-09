# -------- Build stage --------
FROM node:24-slim AS builder

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    make \
    g++ \
 && rm -rf /var/lib/apt/lists/*

RUN npm install -g --prefix /build @google/gemini-cli \
 && npm cache clean --force


# -------- Runtime stage --------
FROM node:24-slim

# Prepend /usr/local/bin but keep base PATH
ENV PATH="/usr/local/bin:${PATH}"

# Copy only what is required for gemini CLI
COPY --from=builder /build/bin /usr/local/bin
COPY --from=builder /build/lib /usr/local/lib

USER node

ENTRYPOINT ["gemini"]

