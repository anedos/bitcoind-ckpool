FROM ubuntu:24.10
LABEL maintainer="Andronikos Nedos <andronikos.nedos@radpool.org>"

# Install dependencies
RUN apt-get update && apt-get install -y \
    git \
    build-essential \
    libssl-dev \
    libcurl4-openssl-dev \
    libjansson-dev \
    yasm \
    libzmq3-dev \
    pkgconf \
    autoconf \
    automake \
    libtool \
    libcap2-bin \
    && rm -rf /var/lib/apt/lists/*

# Clone CKPool repository
RUN git clone https://bitbucket.org/ckolivas/ckpool.git /ckpool

# Build CKPool
WORKDIR /ckpool
RUN ./autogen.sh
RUN ./configure
RUN make
RUN make install
WORKDIR /root
COPY ckpool.conf .

# Expose the CKPool stratum port
EXPOSE 3333

CMD ["ckpool", "-L", "-c ckpool.conf"]