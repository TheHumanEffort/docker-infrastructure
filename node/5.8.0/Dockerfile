FROM node:5.8.0

RUN mkdir -p /application
WORKDIR /application

ENV PORT 8080
EXPOSE 8080

ONBUILD COPY package.json /application
ONBUILD RUN npm install
ONBUILD COPY . /application

CMD [ "npm", "start" ]
