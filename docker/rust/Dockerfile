FROM ubuntu:latest

RUN sed -i 's@archive.ubuntu.com@ftp.jaist.ac.jp/pub/Linux@g' /etc/apt/sources.list

RUN apt-get update && apt-get install -y \
    software-properties-common  vim curl \
    wget build-essential libreadline-dev \ 
    libncursesw5-dev libssl-dev libsqlite3-dev libgdbm-dev \
    libbz2-dev liblzma-dev zlib1g-dev uuid-dev libffi-dev libdb-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/*
   
RUN adduser --disabled-password user
ENV USER user


#Python install
RUN wget --no-check-certificate https://www.python.org/ftp/python/3.11.5/Python-3.11.5.tgz \
    && tar -xf Python-3.11.5.tgz \
    && cd Python-3.11.5 \
    && ./configure --enable-optimizations\
    && make \
    && make install

RUN add-apt-repository universe \
    && apt update \
    &&apt install -y python3-pip

# Rust install
ENV RUST_HOME /usr/local/lib/rust
ENV RUSTUP_HOME ${RUST_HOME}/rustup
ENV CARGO_HOME ${RUST_HOME}/cargo
RUN mkdir /usr/local/lib/rust && \
    chmod 0755 $RUST_HOME
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs > ${RUST_HOME}/rustup.sh \
    && chmod +x ${RUST_HOME}/rustup.sh \
    && ${RUST_HOME}/rustup.sh -y --default-toolchain nightly --no-modify-path
ENV PATH $PATH:$CARGO_HOME/bin
    
RUN apt-get autoremove -y \
    && pip install pipenv \
    && rustup component add rls rust-analysis rust-src rustfmt clippy #\
 #   && cargo install cargo-edit cargo-watch