# specify node base image to build from
FROM node:12.18.2 as build

# specify build argument as an environment variable during build
ARG REACT_APP_SERVICES_HOST=/services/m

# specifiy working directory *within* the Docker container
WORKDIR /app

# copies package.json and its locked counterpart into 
# the container's working directory specified above
COPY ./package.json /app/package.json
COPY ./package-lock.json /app/package-lock.json

# install dependencies and build React.js application
RUN yarn install
COPY . .
RUN yarn build

# set Nginx as our docker base image
FROM nginx

# copy nginx.conf file required for reverse proxy configuration
COPY ./nginx/nginx.conf /etc/nginx/conf.d/default.conf

# copies html built above into the container
COPY --from=build /app/build /usr/share/nginx/html

# Dockerfile only build the image, not the container.
# To check if image exists, use `docker images` command

# To spin up a container instance from an image named 'frontend-image', use 
# `docker run -it --rm -d -p 8080:80 --name ilovecs3219 frontend-image`
# -it: creates interactive bash shell in the container
# --rm: automatically remove the container when it exits
# -d: run container in background and print container ID
# -p: map docker host's port to port within docker
#  --name: specify the container name
