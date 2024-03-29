# get latest go & alpine latest
ARG GO_VERSION=1.22.0
ARG ALPINE_VERSION=3.19

# download the latest image
FROM golang:${GO_VERSION}-alpine${ALPINE_VERSION}

# add non-root user for vscode
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000
# Setup user
RUN adduser $USERNAME -s /bin/sh -D -u $USER_UID $USER_GID && \
    mkdir -p /etc/sudoers.d && \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Install packages and Go language server
RUN apk add -q --update --progress --no-cache git sudo openssh-client
RUN go install -v golang.org/x/tools/gopls@latest 2>&1

# Setup environment for $USERNAME
USER $USERNAME
ENV EDITOR=vi \
    LANG=en_US.UTF-8

# github with ssh is not runnning normally, so let's force  it
# Create a dir to mount host's .ssh folder into to be used by script
RUN mkdir -p /home/$USERNAME/.host_ssh
# copy the script using which we shall force github to run
COPY force_ssh_github /home/$USERNAME/

# Change back user to ROOT
USER root
