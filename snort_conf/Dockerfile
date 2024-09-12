FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    snort \
    libpcap-dev \
    && rm -rf /var/lib/apt/lists/*

COPY snort.conf /etc/snort/snort.conf
COPY local.rules /etc/snort/rules/local.rules

CMD ["snort", "-A", "console", "-c", "/etc/snort/snort.conf", "-i", "eth0"]