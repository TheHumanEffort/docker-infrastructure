FROM node:5.8.0

# Optional: nodemon gives you superpowers to automatically restart
# your server whenever a file changes:
RUN npm install -g nodemon

# Make directories to store the dependencies and the application code:

RUN mkdir -p /package
RUN mkdir -p /application
WORKDIR /application

# Set up the basics:

ENV PORT 8080
EXPOSE 8080

# Tell node where to find dependencies (they are not installed in the
# normal location

ENV NODE_PATH /package/node_modules

# Optional: use a secondary "base" package installation, so that
# incremental updates are faster.  When you copy package to
# .base-package.json, Docker will rebuild from scratch.  Without it,
# every time you change dependencies, node will have to install
# everything as if you'd never installed it at all.

COPY .base-package.json /package/package.json
RUN npm install --prefix /package

# Make incremental updates to the dependencies:

COPY package.json /package/package.json
RUN npm install --prefix /package

# Copy the application

COPY . /application

# By default, run the application with node:

CMD [ "node","index.js" ]

