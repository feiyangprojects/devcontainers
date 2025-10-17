FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

ARG RUNTIME
ARG SDK
ARG VERSION

RUN sudo apt update && \
    sudo apt install --yes software-properties-common && \
    sudo add-apt-repository --yes ppa:flatpak/stable && \ 
    sudo apt install --yes flatpak flatpak-builder gnupg rclone
RUN sudo flatpak remote-add flathub https://flathub.org/repo/flathub.flatpakrepo && \
    sudo flatpak install --assumeyes "$RUNTIME//$VERSION" "$SDK//$VERSION" org.flathub.flatpak-external-data-checker
RUN sudo rm -rf /var/lib/apt/lists/*