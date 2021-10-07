FROM node:latest
MAINTAINER Áquila Tavares
COPY . /var/www
WORKDIR /var/www
RUN npm install
ENTRYPOINT ["npm", "start"]
EXPOSE 3000