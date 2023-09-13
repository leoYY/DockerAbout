FROM ubuntu:20.04
ARG TARGETARCH
ARG DEBIAN_FRONTEND=noninteractive
ARG USERNAME=dev
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*
RUN mv /etc/apt/sources.list /etc/apt/sources.list_bak
COPY sources.list_$TARGETARCH /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade
RUN apt-get install -y git
RUN apt-get install -y sudo
RUN apt-get install -y g++
RUN apt-get install -y cmake
RUN apt-get install -y ccache
RUN apt-get install -y ninja-build
RUN apt-get install -y checkinstall
RUN apt-get install -y wget
RUN apt-get install -y openjdk-8-jdk maven
RUN apt-get install -y tmux
RUN apt-get install -y exuberant-ctags
RUN useradd -p $USERNAME --user-group --system --create-home --no-log-init --shell /bin/bash dev
RUN echo "dev ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/dev

USER $USERNAME
WORKDIR /home/$USERNAME/

RUN git config --global url.https://ghproxy.com/https://github.com/.insteadOf https://github.com/

RUN sh -c "$(curl -fsSL https://ghproxy.com/https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

RUN mkdir /home/$USERNAME/to_remove  \
        && cd /home/$USERNAME/to_remove \
        && git clone https://github.com/leoYY/vimrc \
        && cp vimrc/zsh/.zsh_antigen vimrc/zsh/.zshrc /home/$USERNAME/ \
        && cp -rf vimrc /home/$USERNAME/.vim \
        && rm -rf /home/$USERNAME/to_remove

RUN mkdir -p /home/$USERNAME/.vim/zsh
COPY antigen.zsh /home/$USERNAME/.vim/zsh/

RUN git clone --depth 1 https://github.com/junegunn/fzf /home/$USERNAME/.fzf

RUN sed -i 's/https:\/\/github.com/https:\/\/ghproxy.com\/https:\/\/github.com/' /home/$USERNAME/.fzf/install

RUN /home/$USERNAME/.fzf/install

RUN sudo apt-get install -y language-pack-en

RUN sudo update-locale

RUN zsh -c "source ~/.zshrc"

RUN sudo apt update

RUN sudo apt-get install -y python3-pip python3.10-dev

RUN sudo apt-get install -y neovim

RUN python3 -m pip install pynvim

RUN ln -s ~/.vim/.vimrc .vimrc

RUN git clone --progress https://github.com/vim/vim.git

RUN sudo apt-get install -y libncurses-dev

RUN cd vim && git checkout v9.0.1614 \
        && cd src && ./configure --with-features=huge --enable-python3interp --with-python3-config-dir=/usr/lib/python3.10/config-3.10-x86_64-linux-gnu --enable-cscope --enable-multibyte --enable-fail-if-missing && make -j8 &&  sudo make install

RUN vim --version

RUN sudo apt-get install -y cmake

# ignore vim ret
RUN vim -E -s -u "$HOME/.vimrc" +PlugInstall +qall || echo 0

#RUN zsh -c "sed -i 's/xterm-256/putty-256/' ~/.zshrc"

RUN git clone https://github.com/longld/peda.git ~/peda && echo "source ~/peda/peda.py" >> ~/.gdbinit

# install packer
RUN git clone --depth 1 https://github.com/wbthomason/packer.nvim ~/.local/share/nvim/site/pack/packer/start/packer.nvim

RUN zsh -c "ln -s ~/.vim/.config ~/.config"

#RUN nvim +PackerSync +qa
CMD /bin/zsh
