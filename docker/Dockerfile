FROM ubuntu:18.04

ARG REMOTE_BUILDCACHE_URL="s3://spack-tutorial/mirror/build_cache"
ARG SPACK_PUBLIC_GPG_KEY=""
ARG AWS_ACCESS_KEY_ID="None"
ARG AWS_SECRET_ACCESS_KEY="None"

RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        autoconf \
        ca-certificates \
        curl \
        clang \
        clang-3.7 \
        emacs \
        g++ \
        g++-4.8 \
        gcc \
        gcc-4.8 \
        gfortran \
        gfortran-4.8 \
        git \
        gnupg2 \
        iproute2 \
        make \
        openssh-server \
        python3 \
        python3-pip \
        tcl \
        unzip \
        vim \
        wget && \
    python3 -m pip install --upgrade pip setuptools wheel && \
    python3 -m pip install --upgrade gnureadline && \
    python3 -m pip install --upgrade awscli && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3 1 && \
    apt-get autoremove --purge && \
    apt-get clean

RUN mkdir -p /mirror/build_cache && \
    aws s3 sync ${REMOTE_BUILDCACHE_URL} /mirror/build_cache

COPY /public.key /mirror/public.key
# COPY /packages.yaml /etc/spack/packages.yaml
COPY /tutorial-test.sh /tutorial/.test/tutorial-test.sh

### TODO: Find another way to set perms without increasing the image download size
RUN useradd -ms /bin/bash spack && \
    chmod -R go+r /mirror && \
#    chmod -R go+r /etc/spack && \
    chmod go+rx /tutorial/.test/tutorial-test.sh

USER spack

WORKDIR /home/spack

CMD ["bash"]i

