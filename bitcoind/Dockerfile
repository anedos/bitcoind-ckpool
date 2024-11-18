# Use a lightweight base image
FROM ubuntu:20.04
LABEL maintainer="Andronikos Nedos <andronikos.nedos@radpool.org>"

# Arguments for Bitcoin Core version and signature
ARG VERSION=28.0
ARG ARCH=x86_64
# Wladimir J. van der Laan
ARG BITCOIN_CORE_SIGNATURE=71A3B16735405025D447E8F274810B012346C9A6

# Install dependencies
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    git \
    gpg \
    gnupg \
    libevent-dev \
    libboost-dev \
    libzmq3-dev \
    libssl-dev \
    curl \
    software-properties-common \
    ca-certificates \
    libatomic1 \
    wget \
    && rm -rf /var/lib/apt/lists/*

# import public keys from trusted bitcoin devs
RUN git clone https://github.com/bitcoin-core/guix.sigs
RUN gpg --import guix.sigs/builder-keys/*

# Fetch, verify, and extract Bitcoin Core
RUN cd /tmp \
    && wget https://bitcoincore.org/bin/bitcoin-core-${VERSION}/SHA256SUMS.asc \
    https://bitcoincore.org/bin/bitcoin-core-${VERSION}/SHA256SUMS \
    https://bitcoincore.org/bin/bitcoin-core-${VERSION}/bitcoin-${VERSION}-${ARCH}-linux-gnu.tar.gz \
    && gpg --verify --status-fd 1 --verify SHA256SUMS.asc SHA256SUMS 2>/dev/null | grep "^\[GNUPG:\] VALIDSIG.*${BITCOIN_CORE_SIGNATURE}\$" \
    && sha256sum --ignore-missing --check SHA256SUMS \
    && tar -xzvf bitcoin-${VERSION}-${ARCH}-linux-gnu.tar.gz -C /opt \
    && ln -sv bitcoin-${VERSION} /opt/bitcoin \
    && /opt/bitcoin/bin/test_bitcoin --show_progress \
    && rm -v /opt/bitcoin/bin/test_bitcoin /opt/bitcoin/bin/bitcoin-qt \
    && rm -rf /tmp/*

# Add Bitcoin binaries to PATH
ENV PATH="/opt/bitcoin/bin:$PATH"

# Expose ports for Bitcoin RPC and P2P
EXPOSE 8332 8333

# Set data directory
VOLUME /data

CMD ["bitcoind", "-datadir=/data", "-printtoconsole"]
