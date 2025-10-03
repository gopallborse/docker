FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

# VOLUME ["/app/feedback"] // this is anonymous volume, which gets deleted once the container is shut-off. solution is named volume
# VOLUME ["/app/node_modules"] // this is also anonymous volume, which needs to be there because bind mount removes the node_modules folder while image creation and so container get missing dependencies error. this command can be added in the CLI while running the container

# approach using node, need to restart the server every time the code changes
# CMD [ "node", "server.js" ]

# approach using nodemon, server restarts automatically every time the code changes, start command added in scripts section in package.json file
CMD [ "npm", "start" ] 