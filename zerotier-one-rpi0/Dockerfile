ARG BUILD_FROM=hassioaddons/base:2.3.0
# hadolint ignore=DL3006
FROM hassioaddons/ubuntu-base-armhf:2.2.0 as builder

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV LANG C.UTF-8

RUN apt-get update && apt-get install -y curl gnupg
RUN apt-key adv --keyserver ipv4.pool.sks-keyservers.net --recv-keys 0x1657198823e52a61 \
  && echo "deb http://download.zerotier.com/debian/stretch stretch main" > /etc/apt/sources.list.d/zerotier.list
RUN apt-get update && apt-get install -y zerotier-one=1.2.12

# Build the production container
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

ENV LANG C.UTF-8

# Build arugments
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION
ARG BUILD_ARCH

RUN apk add --update libgcc libstdc++ iproute2 kmod

RUN addgroup --system --gid 1000 zerotier-one \
  && adduser --system --ingroup zerotier-one --home /var/lib/zerotier-one --no-create-home --uid 1000 zerotier-one

COPY --from=builder /var/lib/zerotier-one/zerotier-cli /usr/sbin/zerotier-cli
COPY --from=builder /var/lib/zerotier-one/zerotier-idtool /usr/sbin/zerotier-idtool
COPY --from=builder /usr/sbin/zerotier-one /usr/sbin/zerotier-one

# Copy root filesystem
COPY rootfs /

# Labels
LABEL \
    io.hass.name="zerotier-one" \
    io.hass.description="ZeroTier One for Hass.io" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Francesco Circhetta <francesco.circhetta@gmail.com>" \
    org.label-schema.description="ZeroTier One for Hass.io" \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="ZeroTier" \
    org.label-schema.schema-version="1.0.0" \
    org.label-schema.url="https://github.com/circhioz/hassio-addon-zerotier-rpi0" \
    org.label-schema.usage="https://github.com/circhioz/hassio-addon-zerotier-rpi0/blob/master/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/circhioz/hassio-addon-zerotier-rpi0" \
    org.label-schema.vendor="Francesco's Hass.io Addons"
