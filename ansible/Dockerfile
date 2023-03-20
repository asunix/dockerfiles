FROM debian:11

LABEL maintainer "Giovanni Moroni <asunix@gmail.com>"

ARG UNAME=testuser
ARG UID=1000
ARG GID=1000

RUN DEBIAN_FRONTEND=noninteractive apt-get update -y \
    && DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y python3-pip \
    sshpass \
    && groupadd -g $GID -o $UNAME \
    && useradd -m -u $UID -g $GID -o -s /bin/bash $UNAME \
    && rm -rf /var/lib/apt/lists/*

RUN pip install pip --upgrade; \
    pip install --upgrade virtualenv; \
    pip install pywinrm[kerberos]; \
    pip install pywinrm; \
    pip install jmspath; \
    pip install requests; \
    python3 -m pip install ansible

USER $UNAME

WORKDIR /work
