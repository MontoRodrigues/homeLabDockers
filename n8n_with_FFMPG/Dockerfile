FROM docker.n8n.io/n8nio/n8n:latest

USER root

# 1. Manually reinstall apk-tools because it's missing in v2.x
RUN ARCH=$(uname -m) && \
  wget -qO- "https://dl-cdn.alpinelinux.org/alpine/latest-stable/main/${ARCH}/" | \
  grep -o 'href="apk-tools-static-[^"]*\.apk"' | \
  sed 's/href="//' | sed 's/"//' | xargs -I {} wget "https://dl-cdn.alpinelinux.org/alpine/latest-stable/main/${ARCH}/{}" && \
  tar -xzf apk-tools-static-*.apk && \
  ./sbin/apk.static add --no-cache apk-tools && \
  rm apk-tools-static-*.apk

# 2. Now that 'apk' is restored, install ffmpeg
RUN apk add --no-cache ffmpeg

USER node