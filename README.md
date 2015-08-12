# Installation

## install nvm
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.25.4/install.sh | bash

## install latest nodejs stable
nvm install stable
nvm use stable
nvm ls

## install aglio
npm install -g aglio --python=/usr/bin/python

## generate API
aglio -i docs/API.md -o templates/api.tpl

## Notes:
# API.md contains current production API
# API-experimental.md contains both current and future features