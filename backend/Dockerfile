FROM alpine:latest

WORKDIR /usr/src/app

COPY package*.json ./

RUN apk update

RUN yarn install --production

# RUN npm install

EXPOSE 8001
