FROM ubuntu:20.04

ENV TIMEZONE=GMT
ENV USERNAME=docker

LABEL maintainer="HADMARINE <contact@hadmarine.com>"

SHELL ["/bin/bash", "-c"]

# RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN apt -y update
RUN apt -y install build-essential curl gnupg git zsh nano language-pack-en sudo
RUN update-locale
RUN adduser --shell /bin/zsh --disabled-password --gecos "" ${USERNAME}
RUN adduser ${USERNAME} sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

# RUN chsh -s ~/.zshrc

USER ${USERNAME}
WORKDIR /home/${USERNAME}

RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash \
    # && source ~/.bashrc \
    && source ~/.profile \
    && nvm install node \
    && npm i -g yarn

# ZSH configuration

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

RUN curl -o ~/.zshrc https://gist.githubusercontent.com/HADMARINE/0fb134d56193d1b10be8d985e2e2f9a1/raw/d523a828dfc693ab8258c3f0571ce3c9faa984ea/.zshrc



CMD [ "zsh" ]