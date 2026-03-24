# Stage 1
FROM node:14 as build-stage

WORKDIR /frontendui
COPY package.json .
RUN npm install
COPY . .

ARG REACT_APP_API_BASE_URL
ENV REACT_APP_API_BASE_URL=$REACT_APP_API_BASE_URL

RUN npm run build

# Stage 2
FROM nginx:1.17.0-alpine

<<<<<<< HEAD:booksui/Dockerfile
COPY --from=build-stage /frontendui/build /usr/share/nginx/html
=======
COPY --from=build-stage /frontendui/build /usr/share/nginx/html
>>>>>>> dbc8053 (Renamed subfolders for clarity):frontendui/Dockerfile
EXPOSE $REACT_DOCKER_PORT

CMD nginx -g 'daemon off;'
