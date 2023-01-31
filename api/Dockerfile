FROM node:17-alpine
#tells docker what directory to use
WORKDIR /app

#you need this for the npm install
COPY package.json .

#this layer is to install the dependencies, normally you would do npm install but instead do this similar thing
#you can specify commands as the image gets built
RUN npm install

#it means look inside ./api and then the second . is the path inside the image to copy source code to. above copies code into a folder called app inside the image
COPY . .

#the port is owned by the container, not our computer
# required for docker desktop port mapping
#below is somewhat optional because it works with docker desktop
EXPOSE 4000

#you don't put node run app.js in here because these commands are ran at build time, the image is not supposed to
#be a running application, it is supposed to be the blueprint for a container. the container is what actually runs the application
#CMD allows us to specify any commands that should be ran at runtime
#we write this command as an arrya of strings
CMD ["node", "app.js"]