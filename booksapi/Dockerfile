FROM node:14

WORKDIR /booksapi
COPY package.json .
RUN npm install
COPY . .
CMD npm start
