FROM docker.n8n.io/n8nio/n8n:latest
USER root
RUN set -eux; \
    if command -v apt-get >/dev/null 2>&1; then \
      apt-get update && apt-get install -y --no-install-recommends ffmpeg && \
      rm -rf /var/lib/apt/lists/*; \
    elif command -v apk >/dev/null 2>&1; then \
      apk add --no-cache ffmpeg; \
    else \
      echo "No supported package manager found (expected apt-get or apk)"; \
      exit 1; \
    fi
USER node