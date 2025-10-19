FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

ARG DEBIAN_FRONTEND=noninteractive

COPY . .

RUN VERSION="$(grep -oP 'VERSION_ID="\K[^"]+' '/etc/os-release')" && \
    curl --location "https://download.opensuse.org/repositories/openSUSE:/Tools/xUbuntu_$VERSION/Release.key" | sudo gpg --dearmor --output '/etc/apt/keyrings/opensuse-tools.gpg' && \
    echo "deb [signed-by=/etc/apt/keyrings/opensuse-tools.gpg] https://download.opensuse.org/repositories/openSUSE:/Tools/xUbuntu_$VERSION ./" | sudo tee /etc/apt/sources.list.d/opensuse-tools.list
RUN sudo apt update && \
    sudo apt install debconf-utils && \
    debconf-set-selections debconf && \
    sudo apt install --assume-yes 'osc' 'obs-service-*' 'libxml2-utils'
RUN sudo rm -rf /var/lib/apt/lists/*