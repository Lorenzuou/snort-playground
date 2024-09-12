FROM ubuntu:20.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    hping3 \
    nmap \
    netcat \
    iperf3 \
    siege \
    dnsutils \
    python3 \
    python3-pip \
    iputils-ping \
    && rm -rf /var/lib/apt/lists/*


# Criar um script simples para o ataque Slowloris
RUN echo '#!/usr/bin/env python3\nfrom slowloris import Slowloris\nimport sys\nif len(sys.argv) > 1:\n    Slowloris(sys.argv[1]).attack()' > /usr/local/bin/slowloris-attack.py && \
    chmod +x /usr/local/bin/slowloris-attack.py

WORKDIR /attack

CMD ["/bin/bash"]