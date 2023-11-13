# 42 Devcontainer
# by HADMARINE

FROM ubuntu:22.04

# Set your timezone : https://en.wikipedia.org/wiki/List_of_tz_database_time_zones
ENV TIMEZONE=CET
# If you want to change username, you must change volumes in docker-compose.yml and workspaceFolder in devcontainer.json
ENV USERNAME=devuser

LABEL maintainer="HADMARINE <contact@hadmarine.com>"

SHELL ["/bin/bash", "-c"]

RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN apt update --no-install-recommends -y
RUN apt -y install build-essential git zsh nano language-pack-en sudo lsb-release ca-certificates
RUN mkdir -p /etc/apt/keyrings
RUN apt install --no-install-recommends \
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
    && apt-get autoremove --yes 

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

# 42 Norminette
RUN python3 -m pip install --upgrade pip setuptools && python3 -m pip install norminette

# Add docker
RUN install -m 0755 -d /etc/apt/keyrings
RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
RUN chmod a+r /etc/apt/keyrings/docker.gpg
RUN echo \
    "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
    "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
    tee /etc/apt/sources.list.d/docker.list > /dev/null
RUN apt -y update
RUN apt -y install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Remove c++ Symlink and replace with link to g++
RUN rm /usr/bin/c++ && ln -s /usr/bin/g++ /usr/bin/c++

# Remove installations
RUN rm -rf /var/lib/{apt,dpkg,cache,log}

# Add user
RUN update-locale
RUN adduser --shell /bin/zsh --disabled-password --gecos "" ${USERNAME}
RUN adduser ${USERNAME} sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER ${USERNAME}
WORKDIR /home/${USERNAME}

# SSH Keys
RUN mkdir -p /home/${USERNAME}/.ssh && mkdir -p /home/${USERNAME}/src
COPY ./.ssh/ /home/${USERNAME}/.ssh/

# ZSH configuration
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

RUN curl -o ~/.zshrc https://gist.githubusercontent.com/HADMARINE/0fb134d56193d1b10be8d985e2e2f9a1/raw/d523a828dfc693ab8258c3f0571ce3c9faa984ea/.zshrc

# SSH Keys

CMD [ "zsh" ]