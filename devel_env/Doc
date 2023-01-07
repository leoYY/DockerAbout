FROM ubuntu:20.04
ARG TARGETARCH
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*
RUN mv /etc/apt/sources.list /etc/apt/sources.list_bak
COPY sources.list_$TARGETARCH /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade
RUN apt-get install -y git
RUN apt-get install -y vim
RUN apt-get install -y sudo
RUN apt-get install -y g++
RUN apt-get install -y cmake
RUN apt-get install -y ccache
RUN apt-get install -y ninja-build
RUN apt-get install -y checkinstall
RUN apt-get install -y wget
RUN apt-get install -y python3.9
RUN apt-get install -y python3.9-dev
RUN apt-get install -y openjdk-8-jdk maven
RUN apt-get install -y tmux
RUN apt-get install -y ctags
RUN useradd -p dev --user-group --system --create-home --no-log-init --shell /bin/bash dev
RUN echo "dev ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/dev

USER dev
WORKDIR /home/dev/

CMD ["/bin/bash"]

