FROM debian:buster

SHELL ["/bin/bash", "--login", "-c"]

RUN apt-get update && apt-get install -y -q --no-install-recommends \
  apt-transport-https \
  build-essential \
  ca-certificates \
  curl \
  git \
  libssl-dev \
  libvips-dev \
  libffi-dev \
  wget \
  gnupg2

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /app

ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION 14.4.0

RUN mkdir -p $NVM_DIR

RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
RUN . $NVM_DIR/nvm.sh \
  && nvm install $NODE_VERSION \
  && nvm alias default $NODE_VERSION \
  && nvm use default

ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/v$NODE_VERSION/bin:$PATH

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install -y --no-install-recommends yarn

ENV PATH $HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH
RUN yarn config set prefix ~/.yarn -g

COPY package.json ./
COPY yarn.lock ./

RUN yarn install

COPY . .
