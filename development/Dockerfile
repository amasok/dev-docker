FROM ubuntu:bionic
MAINTAINER takeda takeda@amasok.com
RUN echo ${DEV_USER}


RUN apt-get update -y
RUN chmod go+w,u+s /tmp

# package
RUN apt-get install openssh-server zsh tmux build-essential -y
RUN apt-get install wget unzip curl tree grep bison libssl-dev openssl zlib1g-dev -y
# db client
RUN apt-get install mysql-client redis-tools postgresql-client mongodb-clients -y

#vim
RUN apt-get install git mercurial gettext libncurses5-dev  libperl-dev python-dev python3-dev ruby-dev lua5.2 liblua5.2-dev luajit libluajit-5.1 -y
RUN cd /tmp \
    && git clone https://github.com/vim/vim.git \
    && cd /tmp/vim \
    && ./configure --with-features=huge --enable-perlinterp --enable-pythoninterp --enable-python3interp --enable-rubyinterp --enable-luainterp --with-luajit --enable-fail-if-missing \
    && make \
    && make install

# sshd config
RUN sed -i 's/.*session.*required.*pam_loginuid.so.*/session optional pam_loginuid.so/g' /etc/pam.d/sshd
RUN mkdir /var/run/sshd

# user
RUN echo 'root:root' |chpasswd
RUN useradd -m ${DEV_USER} \
    && echo "${DEV_USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    && echo '${DEV_USER}:${DEV_USER}' | chpasswd
RUN chsh -s /usr/bin/zsh ${DEV_USER}

USER ${DEV_USER}
WORKDIR /home/${DEV_USER}
ENV HOME /home/${DEV_USER}

# # dotfiles
# RUN git clone https://github.com/amasok/dotfiles.git ~/dotfiles \
#     && cd ~/dotfiles \
#     && git checkout os/ubuntu \
#     && zsh link.sh \
#     && zsh zshenv.sh \
#     && zsh tmux.sh \
#     && zsh vimrc.sh \
#     && zsh rbenv.sh 2.5.1 \
#     && zsh pyenv.sh 3.7.0 \
#     && zsh phpenv.sh 7.2.9 \
#     $$ zsh gvm.sh 1.10.2 \


