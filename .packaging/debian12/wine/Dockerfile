FROM foundationdebian12 AS builder

COPY ./ /opt/wine/

SHELL [ "/usr/bin/env", "-S", "bash", "-ex", "-o", "pipefail", "-c" ]

RUN <<EOS
    : build tools
    cp -ra /opt/wine/ /opt/wine64/
    mkdir /opt/wine64/$( dpkg --print-architecture )
EOS

FROM builder

RUN <<EOS
    pushd /opt/wine64/$( dpkg --print-architecture )
    ../configure --enable-win64
    make __tooldeps__ -j `nproc`
    make -C nls
    popd

    : build deb
    pushd /opt/wine
    dpkg-buildpackage -d -b -a arm64 -us -uc -ui
EOS
