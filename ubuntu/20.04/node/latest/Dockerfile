FROM ubuntu:20.04

ENV TIMEZONE=GMT
ENV USERNAME=docker

LABEL maintainer="HADMARINE <contact@hadmarine.com>"

RUN rm /bin/sh && ln -s /bin/bash /bin/sh

RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN apt -y update
RUN apt -y install build-essential curl gnupg git zsh nano language-pack-en sudo
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash -
RUN apt -y install nodejs
RUN npm i -g yarn
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash \
    && export NVM_DIR="$HOME/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" \
    && nvm install node
RUN update-locale
RUN adduser --shell /bin/zsh --disabled-password --gecos "" ${USERNAME}
RUN adduser ${USERNAME} sudo
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

USER ${USERNAME}
WORKDIR /home/${USERNAME}

# ZSH configuration
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
RUN git clone https://github.com/zsh-users/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

RUN curl -o ~/.zshrc https://gist.githubusercontent.com/HADMARINE/0fb134d56193d1b10be8d985e2e2f9a1/raw/507ef09b43016c0b9013a04f8cd71cff8bf3c471/.zshrc

CMD [ "zsh" ]