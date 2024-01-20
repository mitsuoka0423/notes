FROM mcr.microsoft.com/vscode/devcontainers/javascript-node:0-14

WORKDIR /workspace

RUN apt update \
  && apt install git -y

RUN npm i

ENTRYPOINT [ "npm run zenn:preview" ]
