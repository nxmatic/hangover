FROM debian:12

ENV DEBIAN_FRONTEND=noninteractive

RUN cat /etc/apt/sources.list | grep -v "^#"  | sed "s/^deb /deb [arch=amd64] /g" > /tmp/amd64.list && \
    cat /tmp/amd64.list | sed "s/\[arch=amd64\]/[arch=arm64]/g" > /tmp/arm64.list && \
    cat /tmp/amd64.list /tmp/arm64.list > /etc/apt/sources.list && \
    dpkg --add-architecture arm64
RUN cat /etc/apt/sources.list

RUN apt-get update && apt-get install -y --no-install-recommends \
    binutils-aarch64-linux-gnu gcc-aarch64-linux-gnu \
    ca-certificates nano python3 clang screen llvm lld wget curl \
    flex bison \
    libfreetype6-dev \
    libglib2.0-dev \
    libltdl-dev \
    libxcb1-dev \
    libx11-dev  \
    librsvg2-bin \
    git gettext make automake autoconf pkg-config libtool gtk-doc-tools clang \
    dpkg-dev \
    libglib2.0-dev:arm64 \
    libfreetype6-dev:arm64 \
    libltdl-dev:arm64 \
    libxcb1-dev:arm64 \
    libx11-dev:arm64 \
    build-essential libc-dev-arm64-cross less libc-devtools libidn2-0:arm64 \
    libx11-dev libxinerama-dev libfreetype-dev libfontconfig-dev \
    cmake \
    libstdc++6-arm64-cross \
    libstdc++-12-dev-arm64-cross \
    libepoxy-dev:arm64 \
    libsdl2-dev:arm64 \
    ninja-build \
    dpkg-dev fakeroot debhelper dh-make \
    libxcursor-dev:arm64 \
    libxi-dev:arm64 \
    libxshmfence-dev:arm64 \
    libxxf86vm-dev:arm64 \
    libxrandr-dev:arm64 \
    libxrender-dev:arm64 \
    libxfixes-dev:arm64 \
    libxcomposite-dev:arm64 \
    libosmesa-dev:arm64 \
    libdbus-1-dev:arm64 \
    libsane-dev:arm64 \
    libusb-1.0-0-dev:arm64 \
    libv4l-dev:arm64 \
    libpulse-dev:arm64 \
    libgstreamer1.0-dev:arm64 \
    libgstreamer-plugins-base1.0-dev:arm64 \
    libgstreamer-plugins-bad1.0-dev:arm64 \
    libudev-dev:arm64 \
    libunwind-dev:arm64 \
    libsdl2-dev:arm64 \
    libfontconfig1-dev:arm64 \
    libkrb5-dev:arm64 \
    libvulkan-dev:arm64 \
    libgl-dev:arm64 \
    libgnutls28-dev:arm64 \
    libcups2-dev:arm64 \
    libxkbcommon-dev:arm64 \
    libxkbregistry-dev:arm64 \
    libwayland-dev:arm64

SHELL [ "/usr/bin/env", "-S", "bash", "-ex", "-o", "pipefail", "-c" ]

RUN <<EOS
    LATEST_MINGW=$(curl -s https://api.github.com/repos/mstorsjo/llvm-mingw/releases/latest | grep -oP '"tag_name": "\K(.*)(?=")')
    ARCH=$(dpkg --print-architecture)
    case $ARCH in
      amd64) MINGW_ARCH="x86_64" ;;
      arm64) MINGW_ARCH="aarch64" ;;
      armhf) MINGW_ARCH="armv7" ;;
      *) echo "Unsupported architecture: $ARCH" && exit 1 ;;
    esac
    cd /opt
    curl -L -o /dev/stdout -s https://github.com/mstorsjo/llvm-mingw/releases/download/${LATEST_MINGW}/llvm-mingw-${LATEST_MINGW}-ucrt-ubuntu-20.04-${MINGW_ARCH}.tar.xz |
      tar -xJf /dev/stdin
    ln -fs llvm-mingw-${LATEST_MINGW}-ucrt-ubuntu-20.04-${MINGW_ARCH} llvm-mingw
    echo "export PATH=/opt/llvm-mingw/bin:\$PATH" >> /etc/bash.bashrc
EOS

ENV PATH="/opt/llvm-mingw/bin:${PATH}"
