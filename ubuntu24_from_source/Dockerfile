ARG ROOT_VERSION=6.30.00
ARG CMAKE_CXX_STANDARD=17

FROM ubuntu:24.04

LABEL maintainer.name="Carlo Mancini Terracciano"
LABEL maintainer.email="carlo.mancini-terracciano@uniroma1.it"

ENV LANG=C.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

COPY packages packages

RUN apt-get update -qq \
    && ln -sf /usr/share/zoneinfo/UTC /etc/localtime \
    && apt-get -y install --no-install-recommends $(cat packages) wget git dpkg-dev ca-certificates \
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

ARG ROOT_VERSION
ARG CMAKE_CXX_STANDARD

RUN ROOT_GIT_URL=https://github.com/root-project/root.git \
    && arch="$(dpkg-architecture -qDEB_HOST_MULTIARCH)" \
    && if git ls-remote --exit-code --heads "$ROOT_GIT_URL" "$ROOT_VERSION" >/dev/null 2>&1; then \
        ROOT_GIT_REVISION="$ROOT_VERSION"; \
    else \
        ROOT_GIT_REVISION="v$(echo "$ROOT_VERSION" | cut -d. -f1)-$(echo "$ROOT_VERSION" | cut -d. -f2)-$(echo "$ROOT_VERSION" | cut -d. -f3)"; \
    fi \
    && git clone --branch "$ROOT_GIT_REVISION" --depth=1 "$ROOT_GIT_URL" /tmp/source \
    && cmake -S /tmp/source -B /tmp/build -G Ninja \
        -DCMAKE_CXX_STANDARD="$CMAKE_CXX_STANDARD" \
        -Dgnuinstall=ON \
        -Dsoversion=ON \
        -Dmathmore=ON \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_LIBDIR="lib/${arch}" \
        -DCMAKE_INSTALL_CMAKEDIR="lib/${arch}/cmake/ROOT" \
        -DCMAKE_INSTALL_INCLUDEDIR=include/ROOT \
        -DCMAKE_INSTALL_PYTHONDIR=lib/python3/dist-packages \
        -DCMAKE_INSTALL_DATADIR=share/root \
        -DCMAKE_INSTALL_SYSCONFDIR=/etc/root \
    && ninja -C /tmp/build install \
    && rm -rf /tmp/build /tmp/source

CMD ["root", "-b"]