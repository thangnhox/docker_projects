# -------- Build stage --------
FROM --platform=linux/arm64 node:24-slim AS builder

RUN apt-get update && apt-get install -y --no-install-recommends \
    python3 \
    make \
    g++ \
 && rm -rf /var/lib/apt/lists/*

RUN npm install -g @google/gemini-cli \
 && npm cache clean --force


# -------- Runtime stage --------
FROM --platform=linux/arm64 node:24-slim

# Prepend /usr/local/bin but keep base PATH
ENV PATH="/usr/local/bin:${PATH}"

# Copy only what is required for gemini CLI
COPY --from=builder /usr/local/bin/gemini /usr/local/bin/gemini
COPY --from=builder /usr/local/lib/node_modules/@google /usr/local/lib/node_modules/@google

ENTRYPOINT ["gemini"]

