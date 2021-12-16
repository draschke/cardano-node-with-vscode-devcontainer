# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/v0.205.2/containers/debian/.devcontainer/base.Dockerfile

# [Choice] Debian version (use bullseye or stretch on local arm64/Apple Silicon): bullseye, buster, stretch
ARG VARIANT="buster"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID
ENV npm config set registry https://registry.npmjs.org/

WORKDIR /
COPY .devcontainer/ /.devcontainer/

RUN ls -la /.devcontainer/*

# Alter node user as needed, install tslint, typescript. eslint is installed by javascript image
RUN if [ "$USER_GID" != "1000" ] || [ "$USER_UID" != "1000" ]; then \
    groupmod --gid $USER_GID $USERNAME \
    && usermod --uid $USER_UID --gid $USER_GID $USERNAME \
    && chown -R $USER_UID:$USER_GID /home/$USERNAME; \
    fi

# [Optional] Uncomment this section to install additional OS packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends apt-utils dialog 2>&1 \
    && apt-get dist-upgrade \
    && apt-get update -y \
    && apt-get -y install libsecret-1-dev \
    && apt-get -y install acl \
    && apt-get -y install nano

RUN sudo chown -R ${USERNAME}:${USERNAME} .devcontainer/scripts/ \
    && chmod +x -R .devcontainer/scripts/
RUN ./.devcontainer/scripts/4-helm
RUN ./.devcontainer/scripts/6-python
# RUN ./.devcontainer/scripts/5-docker