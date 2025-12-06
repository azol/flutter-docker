# hadolint global ignore=DL3008,DL4006
FROM debian:12-slim

RUN set -eux \
 && groupadd -g 1000 flutter \
 && useradd -u 1000 -g flutter -s /bin/sh -m flutter

RUN set -eux \
 && DEBIAN_FRONTEND=noninteractive apt-get update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
      ca-certificates \
      curl \
      git \
      jq \
      unzip \
      xz-utils \
 && rm -rf /var/lib/apt/lists/*

ARG FLUTTER_HOME=/opt/flutter
ENV PATH="${FLUTTER_HOME}/bin:${PATH}"

RUN set -eux \
 && echo "export PATH=\"${FLUTTER_HOME}/bin:\$PATH\"" > /etc/profile.d/flutter.sh \
 && ln -sf "${FLUTTER_HOME}/bin/flutter" /usr/local/bin/flutter \
 && ln -sf "${FLUTTER_HOME}/bin/dart"    /usr/local/bin/dart

ARG FLUTTER_VERSION=3.38.4
ENV FLUTTER_VERSION="${FLUTTER_VERSION}"

RUN set -eux \
 && if echo "${FLUTTER_VERSION}" | grep -q "pre"; then CHANNEL=beta; else CHANNEL=stable; fi \
 && ARCHIVE="$(curl -fsSL https://storage.googleapis.com/flutter_infra_release/releases/releases_linux.json \
      | jq -r --arg ver "${FLUTTER_VERSION}" --arg chan "${CHANNEL}" \
        '.releases[] | select(.version == $ver and .channel == $chan) | .archive')" \
 && [ -n "${ARCHIVE}" ] \
 && mkdir -p "${FLUTTER_HOME}" \
 && curl -fsSL "https://storage.googleapis.com/flutter_infra_release/releases/${ARCHIVE}" \
      | tar -xJ --strip-components=1 -C "${FLUTTER_HOME}"

USER flutter
WORKDIR /app

RUN ls -l /opt/

RUN ls -l /opt/flutter/

CMD ["/bin/sh"]

LABEL org.opencontainers.image.title="Flutter SDK" \
      org.opencontainers.image.description="Flutter is an open source framework for building beautiful, natively compiled, multi-platform applications from a single codebase." \
      org.opencontainers.image.licenses="BSD-3-Clause" \
      org.opencontainers.image.vendor="Artem Zolochevskii" \
      org.opencontainers.image.authors="Artem Zolochevskii"
