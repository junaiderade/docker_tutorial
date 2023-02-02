Purpose:

- notate everying in Docker Crash Course
- Linux containers are technologies that allow you to package and isolate applications with their entire runtime environment—all of the files necessary to run.

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
