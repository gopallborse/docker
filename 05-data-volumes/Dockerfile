FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

# VOLUME ["/app/feedback"] // this is anonymous volume, which gets deleted once the container is shut-off. solution is named volume
# VOLUME ["/app/node_modules"]

# CMD [ "node", "server.js" ]
CMD [ "npm", "start" ]