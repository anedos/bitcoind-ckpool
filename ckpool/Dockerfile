FROM ubuntu:24.10

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
    && rm -rf /var/lib/apt/lists/*

# Clone CKPool repository
RUN git clone https://bitbucket.org/ckolivas/ckpool.git /ckpool

# Build CKPool
WORKDIR /ckpool
RUN ./autogen.sh
RUN ./configure
RUN make

# Expose the CKPool stratum port
EXPOSE 3333

CMD ["./ckpool"]
