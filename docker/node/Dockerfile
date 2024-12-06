ARG  TAG=latest
ARG ENTRYPOINT

FROM node:${TAG}

WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .

ENTRYPOINT ${CHANGE_ME}
