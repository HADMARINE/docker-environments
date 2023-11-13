#################
# 42 Devcontainer
# By HADMARINE
# With reference of https://github.com/opsec-infosec/42-Devcontainer
#
FROM ubuntu:22.04

LABEL maintainer="HADMARINE <contact@hadmarine.com>"

# Suppress an apt-key warning about standard out not being a terminal. Use in this script is safe.
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn
ENV USERNAME=devuser

# Standard Linux Packages
RUN apt-get update --no-install-recommends -y
RUN apt-get install --no-install-recommends \
    # Standard Build Environment
    'man-db' \
    'less' \
    'build-essential' \
    'libtool-bin' \
    'valgrind' \
    'gdb' \
    'automake' \
    'make' \
    'ca-certificates' \
    'g++' \
    'libtool' \
    'pkg-config' \
    'manpages-dev' \
    'zip' \
    'unzip' \
    'python3' \
    'python3-pip' \
    'git' \
    'openssh-server' \
    'dialog' \
    'llvm' \
    'clang' \
    'curl' \
    'wget' \
    'zsh' \
    'nano' \
    'vim' \
    'moreutils' \
    # Push Swap Projects
    'python3-tk' \
    'ruby' \
    'bc' \
    'htop' \
    # Minishell Projects
    'libreadline-dev' \
    # Minilibx Projects
    'libbsd-dev' \
    'libxext-dev' \
    'libx11-dev' \
    # IRC Project Test Example
    'irssi' \
    'netcat' \
    'tcpdump' \
    'net-tools' \
    #"wireshark" \
    -y \
    && apt-get clean autoclean \
    && apt-get autoremove --yes \
    && rm -rf /var/lib/{apt,dpkg,cache,log}/

# 42 Norminette
RUN python3 -m pip install --upgrade pip setuptools && python3 -m pip install norminette

# OhMyZsh Install, set prompt to DEVCONTAINER
RUN sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"  \
    && echo 'PROMPT=%B%F{blue}[DEVCONTAINER]%f%b$PROMPT' >> /root/.zshrc

# ZSH configuration
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
RUN curl -o ~/.zshrc https://gist.githubusercontent.com/HADMARINE/0fb134d56193d1b10be8d985e2e2f9a1/raw/d523a828dfc693ab8258c3f0571ce3c9faa984ea/.zshrc


# # Add Return Code in prompt for bash
# ENV PROMPT_COMMAND='RET=$?; echo -n "[$RET] "'

# Add clangd
RUN wget https://github.com/clangd/clangd/releases/download/15.0.6/clangd-linux-15.0.6.zip && unzip clangd-linux-15.0.6.zip && cp ./clangd_15.0.6/bin/clangd /usr/local/bin && cp -rd ./clangd_15.0.6/lib/clang/ /usr/local/lib/ && rm -rf ./clangd_15.0.6 && rm clangd-linux-15.0.6.zip
COPY ./.devcontainer/settings.json /root/.vscode-server/data/Machine/settings.json

# minilibx-linux source and install
RUN git clone https://github.com/42Paris/minilibx-linux.git /usr/local/minilibx-linux
RUN cd /usr/local/minilibx-linux/ && ./configure \
    && cp /usr/local/minilibx-linux/*.a /usr/local/lib \
    && cp /usr/local/minilibx-linux/*.h /usr/local/include \
    && cp -R /usr/local/minilibx-linux/man/* /usr/local/man/ \
    && /sbin/ldconfig

# SSH Keys
RUN mkdir -p /home/vscode/src && mkdir -p /root/.ssh
COPY ./.ssh/ /root/.ssh/

# Remove c++ Symlink and replace with link to g++
RUN rm /usr/bin/c++ && ln -s /usr/bin/g++ /usr/bin/c++

# # Export Display for XServer Forwarding
# RUN echo "export DISPLAY=host.docker.internal:0.0" >> /root/.bashrc && echo "export DISPLAY=host.docker.internal:0.0" >> /root/.zshrc

# set working directory to /home/vscode/src
USER ${USERNAME}
WORKDIR /home/${USERNAME}
COPY ./src/ ./src/
WORKDIR /home/${USERNAME}/src



CMD [ "zsh" ]