FROM node

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 80

ENV MONGODB_USERNAME=admin
ENV MONGODB_PASSWORD=secret
ENV MONGODB_DATABASE=database

# CMD [ "node", "app.js" ]
CMD [ "npm", "start" ]