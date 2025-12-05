# hadolint global ignore=DL3008,DL4006
FROM debian:12-slim

ARG FLUTTER_VERSION=3.38.4

ENV FLUTTER_HOME=/opt/flutter
ENV PATH="${FLUTTER_HOME}/bin:${PATH}"

RUN set -eux \
 && groupadd -g 1000 flutter \
 && useradd -u 1000 -g flutter -s /bin/sh -m flutter

RUN set -eux \
 && DEBIAN_FRONTEND=noninteractive apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
      ca-certificates curl xz-utils git jq \
 && rm -rf /var/lib/apt/lists/*

RUN set -eux \
 && if echo "$FLUTTER_VERSION" | grep -q "pre"; then CHANNEL=beta; else CHANNEL=stable; fi \
 && ARCHIVE="$(curl -fsSL https://storage.googleapis.com/flutter_infra_release/releases/releases_linux.json \
      | jq -r --arg ver "$FLUTTER_VERSION" --arg chan "$CHANNEL" \
        '.releases[] | select(.version == $ver and .channel == $chan) | .archive')" \
 && [ -n "$ARCHIVE" ] \
 && curl -fsSL "https://storage.googleapis.com/flutter_infra_release/releases/${ARCHIVE}" \
      | tar -xJ -C /opt

USER flutter
WORKDIR /app

CMD ["/bin/sh"]

