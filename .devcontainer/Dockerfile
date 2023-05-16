ARG VARIANT=ubuntu-20.04
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

RUN apt update && apt install -y p7zip-full python3-pip zstd cmake
RUN pip install --upgrade conan==1.58
