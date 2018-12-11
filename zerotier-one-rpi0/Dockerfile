ARG BUILD_FROM=hassioaddons/base:2.3.0
FROM ${BUILD_FROM}

# Build arugments
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION
ARG BUILD_ARCH
ARG ZT_VER

# Install ZeroTier One
RUN apt-get update -yqq \
  && apt-get install kmod iproute -y \
  && curl https://download.zerotier.com/debian/stretch/pool/main/z/zerotier-one/zerotier-one_${ZT_VER}_${BUILD_ARCH}.deb -o /tmp/zerotier-one.deb \
  && dpkg-deb -x /tmp/zerotier-one.deb /tmp/zerotier-one \
  && cp /tmp/zerotier-one/usr/sbin/zerotier-one /usr/bin \
  && ln -s /usr/bin/zerotier-one /usr/bin/zerotier-cli \
  && addgroup --system --gid 1000 zerotier-one \
  && adduser --system --ingroup zerotier-one --home /var/lib/zerotier-one --no-create-home --uid 1000 zerotier-one \
  && rm -rf /tmp/*

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