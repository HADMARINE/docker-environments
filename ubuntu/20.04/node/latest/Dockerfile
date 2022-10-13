FROM ubuntu:20.04

ENV TIMEZONE=GMT
ENV USERNAME=docker

LABEL maintainer="HADMARINE <contact@hadmarine.com>"

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN apt -y update
RUN apt -y install build-essential curl gnupg git zsh nano language-pack-en sudo
RUN update-locale
RUN adduser --shell /bin/zsh --disabled-password --gecos "" ${USERNAME}
RUN adduser ${USERNAME} sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

RUN chsh -s ~/.zshrc

USER ${USERNAME}
WORKDIR /home/${USERNAME}


ENV NVM_DIR /home/${USERNAME}/.nvm

RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

RUN curl -o ~/.zshrc https://gist.githubusercontent.com/HADMARINE/0fb134d56193d1b10be8d985e2e2f9a1/raw/460d25fd13e6bb8952acbf0953cc67d7d1fd0c55/.zshrc


RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | zsh \
    && export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" \
    && [ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion \
    # && source ~/.bashrc \
    && nvm install node \
    # && export NODE_PATH=$NVM_DIR/v$NODE_VERSION/lib/node_modules \
    # && export PATH=$NVM_DIR/v$NODE_VERSION/bin:$PATH \
    && npm i -g yarn

# ZSH configuration

CMD [ "zsh" ]