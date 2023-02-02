**Tutorial Complete**

Purpose:

- notate everying in Docker Crash Course
- Linux containers are technologies that allow you to package and isolate applications with their entire runtime environment—all of the files necessary to run.

Thoughts:

- i feel like at this point if i learn mjv i know what i need to know to feel confident as a dev
- look up difference between container orchestration and autoscaling/load balancing

<h2>Info</h2>

- container related services for AWS: https://aws.amazon.com/containers/services/
- There are still servers in serverless, but they are abstracted away from app development. A cloud provider handles the routine work of provisioning, maintaining, and scaling the server infrastructure. Developers can simply package their code in containers for deployment.
- ELI5 of K8s
    Kubernetes is a container orchestration system. There are two components to that, and both deliver value in different ways.

    Containerization refers to the practice of isolating software in an operating environment with lower overhead than running a full virtual machine. It takes advantage of some cool features in the Linux kernel to provide a level of isolation from the host operating system and other containers. Isolation is nice in software because you can completely control and reproduce the environment your software runs in, leading to fewer bugs in production.

    Because containers are so lightweight, you can ship them around as a deployable unit from your development, to staging, and eventually production environments. No matter where you run the container, you’re running the exact same system, complete with the same OS, libraries, and configuration.

    Now that we have this cool new abstraction, the question is how do we run it in production, in other words orchestrate. This is where Kubernetes and other solutions come in. Kubernetes handles the job of taking your container unit, and running it on servers operating in a cluster.

    The concept is pretty cool. You set up a cluster of Kubernetes nodes, and you tell the system to “run your container”, and Kubernetes will determine where to run that container. If you configure auto-scaling, Kubernetes will react to increased load by adding more containers and load balancing across them, then scale things back when load drops. It can also auto-heal, by replacing a failed container with a new one.

    By making the system into a lightweight deployable unit, we can really easily launch and destroy a fully reproducible application, and Kubernetes helps by giving us great features taking advantage of that core capability.

<h2>#1 what is docker</h2>
https://www.youtube.com/watch?v=31ieHmcTUOk

- example use case:
    - you are in a dev environemnt and ur making a nodeJS application with a very specific version of node required to run
    - then u share it with another dev, but the thing is that dev would need to set up their dev environment with urs to make sure the application runs correctly. they also need to set up environment variables.
- think of a **container** as a box or package that contains EVERYTHING our application needs to run. All source code, dependencies, runtime environment, etc
- it does not matter what version of python or whatever is installed on my computer because everything i need is in the docker container
- the only thing you need is docker to manage the containers
- it also makes deployment easier. you don't need to configure the server cuz all the config and setup is in the container already.
- VMs are different than containers
    - VMs have their own full OS with its own kernel and are typically slower
    - containers share the hosts operating system and are typically quicker
        - more lightweight
        - use host machine's OS under the hood

<h2>#2 Installing Docker</h2>
https://www.youtube.com/watch?v=8Ev1aXl7TGY&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=2&t=5s

- pretty simple for mac

<h2>#3 Images and Containers</h2>
https://www.youtube.com/watch?v=hhfrFvuHRPU&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=3

- **Images** are like blueprints for containers
    - rumetime env (ex: node)
    - app code
    - any dependencies
    - extra config (environment variables)
    - commands (ex: starting the app)
- images are read only, which means they cannot be changed
    - if you need to change something about an image, you will need to create a brand new image
- when an image is ran, it creates a **container**, a process which can run our application exactly as outlined in the image. A contianer is an **isolated process** which means it runs independently of any other process on your computer.

<h2>#4 Parent Images and Docker Hub</h2>
https://www.youtube.com/watch?v=ZVQmnziXEpA&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=4

- images are made up of different layers
    - each layer essentially adds something new to the image incrementally
    - order of layers does matter
    - typically we start with a "parent image" as the first layer in our image
        - includes the OS and sometimes also the runtime environment
    - the next layers are built on top of the parent image and can be anything else such as:
        - source code
        - dependencies
        - initial scripts which need to run
- when you work with docker images, you also have to work with dockerhub
- dockerhub is an online repo of docker images, its a bit like github
- it contains premade parent images
- ex: if you want to run a docker image in node, you need a parent node image
- u signed up on docker hub with junebugdev
- u ran docker pull node in terminal

<h2>#5 The Dockerfile</h2>
https://www.youtube.com/watch?v=G07FcRhYB2c&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=5

- a dockerfile is a set of instructions to create a dockerimage
- you download the code from the lesson 5 branch on his github
    - https://github.com/iamshaunjp/docker-crash-course/tree/lesson-5
- normally you first cd into api and did npm install to install the dependences which then go into the node modules folder! all dependencies are stored here!
- but we don't wanna do above. we want to run the application in an isolated container with its own version of node
- to do that we have to make an image, which will contain the initial parent image layer to say wha tversion of node/linux wwe want to use adn then extra layers to copy the source code and all the dependencies
- we have to make a dockerfile to make an image
- just write Dockerfile with no extension to make one
- it is helpful to install the docker extension for vs code
- the copy first dot is a relative path (current path) 
- run this: docker build -t myapp . , the - t is to name. the . is a relative path
- once you run the docker file, you don't see anythign indicating that its done or a new file or anything
- it is stored away on a special folder on our computer
- but we will see it on docker desktop

<h2>#6 dockerignore</h2>
https://www.youtube.com/watch?v=UHWCkDbN0yM&list=PLqyUgadpThTJQs0Xa41PoQqD3u4DD1p2M&index=6

- ideally we don't want docker to copy the node modules folder if there is one because we are giving docker the instruction to install a package
- we can get around that by using a docker ignore file

<h2>#7 starting and stopping containers</h2>
https://www.youtube.com/watch?v=ZPEpreOpqao&list=PLqyUgadpThTJQs0Xa41PoQqD3u4DD1p2M&index=7

- you can run containers from docker desktop
- if you run a server you can't just easily access thru browser because remember you are not running the server from your computer but rather from a container,
however docker has a workaround which can be found from docker desktop. You can map the container port to a localhost port
- the above option will only be there if you added the expose instruction in your docker file
- run on dockerhub!
- you can also start and stop containers from terminal
- if you do docker images in terminal you can see the list of images you have
- to run an image do docker run myapp. you can add flags/options
- docker ps will give you a list of runnign containers
- docker stop myapp will stop your container
- docker run --name myapp_c2 -p 4000:4000 -d myapp will run your container on port 4000, -d will detach your terminal from the container
- docker run makes a new container, docker start starts an existing container

<h2> #8 Layer Caching</h2>
https://www.youtube.com/watch?v=_nMpndIyaBU&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=8

- we can make an improvement to the process with **layer caching**
- every line written in the docker file adds a layer
- every additional layer will take docker additional time
- if you change a layer, then docker has to rebuild the layers below it
- docker will cache the layers that don't change. when you build an image, docker looks for an existing image in the cache
 
 <h2> #9 Managing Images and Containers </h2>
https://www.youtube.com/watch?v=4XsjXscp70o&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=9

- if you type docker images then you see all images
- if you type docker ps then you see all containers running
- if you type docker ps -a then you see all containers in general
- to delete a docker image do docker image rm myapp (if my app is the name)
    - make sure you delete containers related to that docker image
- to convert images
- docker system prune -a deletes all images
- builds an image with tag name of v1
    - docker build -t project_12:v1 .
- if we want to run a specific image based on tag we can do
docker run --name myapp_c -p 4000:4000 project_12:v1

<h2> #10 Volumes </h2>
https://www.youtube.com/watch?v=Wh4BcFFr6Fc&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=10

- once an image is made it becomes read only, whether you change the source code or dependencies you have to rebuild the image
- to start a container you can use docker start myapp_c
- you can also use docker stop myapp_c to stop
- it would be annoying to have to build a new container each time you make a change and then have to run that container, which is why docker has 
**volumes** which are a feautre of docker that allow us to specify folders on our host computer that can be made available to running containers
- we can map those folders on our host computer to specific folders inside the cotnainer so that if something changed inside the folder, that chane would also
be reflected inside the folders we mapped to in the container
- volumes just map folders on your computer to folders in the container but they do not change the image itself
- if we wanted to update the image to share with others or to create new containers then you'd have to rebuild the image using docker build again 
- the reason volumes are helpful is because you can see the changes you make without building anything
- nodemon -L app.js is the command ur using to run, you need the -L because it alters the way nodemon works with docker 
- you need this inside the script if you're working with windows, otherwise it won't work
    - he mentioned nodemon is helpful during dev but probably not something you'd need in production
- this will remove a container once you stop it:
    - docker run --name myapp_c_nodemon -p 4000:4000 --rm project_12:nodemon 
- now the thing is nodemon watches for changes to the app.js in the container and it restarts the server when it detects one. without nodemon, it wouldn't do that.
- note that nodemon watches the app.js files in the container, not what's on your computer. which is why volumes need to come into play
- to run a container with a volume
    - docker run --name myapp_c_nodemon -p 4000:4000 --rm -v /Users/junaidmohamed/Documents/code/projects/project_12:/app project_12:nodemon 
    - right click on panel in vs code and hit cpy path
    - above maps project folder on our computer to the app folder in the container
    - a  problem!!!: 
        - you delete node_modules from your project folder and it gets deleted on the running container BUT your container NEEDS the node modules
- in order to prevent above problem we need to map an anonymous volume
docker run --name myapp_c_nodemon -p 4000:4000 --rm -v /Users/junaidmohamed/Documents/code/projects/project_12:/app -v /app/node_modules project_12:nodemon 
- this volume specifies the location of the node modules folder inside the container itself when its running and it doesn't map
that folder to a specific folder on our computer. it maps to itself so it doesn't look at your local files.
- docker compose in the future makes this easier

<h2> #11 Docker Compose </h2>
https://www.youtube.com/watch?v=TSySwrQcevM&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=11

- its annoying to type in super long command from last video in terminal
- sometimes we might multiple projects and we might want to run all those containers at once
- imagine you want to runa  front end, back end, and db all in seperate containers
- docker compose is a tool that comes fully baked into docker itself
- docker compose gives us a way to make a single docker compose file which contains all the container config of our projects
- all the info is in the docker compose file
- docker system prune to delete all containers/images
- put docker-compose up in terminal to run docker compose
- docker-compose down --rmi all -v to remove all containers amd images and volumes created

<h2> #12 Dockerizing a React App </h2>
https://www.youtube.com/watch?v=QePBbG5MoKk&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=12

- you add to the docker compose file in this tutorial and you add the myblog folder
- you can visit localhost:3000 after to see the app taking data from the backend

<h2> #13 Sharing Images on Docker Hub </h2>
https://www.youtube.com/watch?v=YS35VHsbS-0&list=PL4cUxeGkcC9hxjeEtdHFNYMtCpjNBm3h7&index=13

- you registered with dockerhub with junebugdev@gmail.com
- teaches you to push image to repo
- do docker build -t junebugdev/myapi . 
- you didn't do this cuz you didn't feel like it but its pretty intuitive