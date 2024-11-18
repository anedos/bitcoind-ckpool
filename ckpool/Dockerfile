FROM ubuntu:20.04

# Install dependencies
RUN apt-get update && apt-get install -y \
    git \
    build-essential \
    libssl-dev \
    libcurl4-openssl-dev \
    libjansson-dev \
    && rm -rf /var/lib/apt/lists/*

# Clone CKPool repository
RUN git clone https://github.com/ckolivas/ckpool.git /ckpool

# Build CKPool
WORKDIR /ckpool
RUN make

# Expose the CKPool stratum port
EXPOSE 3333

CMD ["./ckpool"]
