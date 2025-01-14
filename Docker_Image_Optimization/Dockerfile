# PULL the base image of ubuntu 
# This ubuntu image is over kill or over specified 
# We should use the slim or alpine tag while pulling the ubuntu image from the docker-hub.com
# FROM ubuntu:latest

# But in the alpine image there will be no pseudo code , no editor like vim or nano etc. Because this image is very purpose driven image.
# Selecting slim or alpine images is a good example of a minimal docker image that provide all of the OS utilities you would expect from a linux container.

# Alpine or Slim images will be reduced images
# Scratch images will be empty images

# All these 7 layers will be cached if something not change in the code

FROM node:22.13.0-alpine

# Install or Update the modules
RUN apt update && \
    apt install -y curl && \
    curl -sL https://deb.nodesource.com/setup_22.x | bash - && \ 
    apt install -y nodejs

WORKDIR /app

# If something change in the package.json or package-lock.json file so all the down stream layers will be cached invalidate.
COPY package.json package.json
COPY package-lock.json package-lock.json  

RUN npm install

# If Something is change in the code then all the down stream layers from the COPY cmd will invalidate the cached.  
# Suppose if we change somthing in the code so we don't want that on every build the package.json and package-lock.json will be installed or this time-taking process will happen
# Copy the source code from host machine and paste it to the container /app root
COPY . .

RUN npm run build 

# How to save additional space ; After build the code remove the whole src dir along with tsconfig.json file. 
RUN rm -rf src/
RUN rm -rf tsconfig.json

CMD ["npm", "start"]
