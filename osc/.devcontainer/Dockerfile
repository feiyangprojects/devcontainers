FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

ARG DEBIAN_FRONTEND=noninteractive

COPY . .

RUN VERSION="$(grep -oP 'VERSION_ID="\K[^"]+' '/etc/os-release')" && \
    curl --location "https://download.opensuse.org/repositories/openSUSE:/Tools/xUbuntu_$VERSION/Release.key" | sudo gpg --dearmor --output '/etc/apt/keyrings/opensuse-tools.gpg' && \
    echo "deb [signed-by=/etc/apt/keyrings/opensuse-tools.gpg] https://download.opensuse.org/repositories/openSUSE:/Tools/xUbuntu_$VERSION ./" | sudo tee /etc/apt/sources.list.d/opensuse-tools.list
RUN sudo apt update && \
    sudo apt install --assume-yes debconf-utils libssl-dev libzstd-dev pkg-config && \
    debconf-set-selections debconf && \
    sudo apt install --assume-yes 'osc' 'obs-service-*' 'libxml2-utils'
RUN sudo apt install --assume-yes python3-libarchive-c && \
    curl -o /lib/obs/service/go_modules https://raw.githubusercontent.com/openSUSE/obs-service-go_modules/refs/heads/master/go_modules && \
    curl -o /lib/obs/service/go_modules.service https://raw.githubusercontent.com/openSUSE/obs-service-go_modules/refs/heads/master/go_modules.service && \
    chmod +x /lib/obs/service/go_modules
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y --profile minimal --default-toolchain stable && \
    . "$HOME/.cargo/env" && \
    git clone --branch="$(curl 'https://api.github.com/repos/openSUSE-Rust/obs-service-cargo/releases/latest' | jq -r '.tag_name')" --depth=1 https://github.com/openSUSE-Rust/obs-service-cargo.git && \
    cd obs-service-cargo && \
    cargo build --release && \
    sudo install -m0644 cargo_vendor.service /lib/obs/service && \
    sudo install -m0755 target/release/cargo_vendor /lib/obs/service && \
    if [ -f 'target/release/cargo_audit' ]; then sudo install -m0755 target/release/cargo_audit /lib/obs/service; fi && \
    cd .. && \
    rm -fr obs-service-cargo

RUN . "$HOME/.cargo/env" && \
    sudo apt remove -y libssl-dev libzstd-dev && \
    rustup self uninstall -y && \
    sudo rm -rf /var/lib/apt/lists/*
