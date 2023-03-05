# Week 1 — APP CONTAINERIZATION!!!

## Brief intro on Containeriaztion concepts...
Containerized applications are applications that run in isolated runtime environments called containers.
- Containers encapsulate an application with all its dependencies, including code, runtime, system tools, system libraries, binaries, configuration files and settings. 
- This all-in-one packaging makes an application portable by enabling it to behave consistently across different hosts—allowing developers to write once and run almost anywhere.
- These containerizations are made possible with the help of container management softwares like Docker, Kubernetes, AWS Fargate, Amazon ECS, etc.

For this particular task, we made use of Docker containerization tool which is one of the best container management services.

Refer to this link for a more detailed explanation of Docker. [Docker overview](https://docs.docker.com/get-started/overview/)

Some of the most important terms associated with it are containers, images, package, deploy, ship, etc.

Docker is used mainly because it has the ability to reduce the size of the deployment by providing a smaller footprint on the operating system via containers.

## IMPLEMENTATION & STEPS INVOLVED!!

- From my Github project repo [AWS BOOTCAMP CRUDDUR 2023](https://github.com/Pascalpedro/aws-bootcamp-cruddur-2023), 
i logged into my [gitpod CDE](https://gitpod.io/#https://github.com/Pascalpedro/aws-bootcamp-cruddur-2023) which i have already setup with the necessary settings such as the *ENVIROMENTAL VARIABLES*, *GIT PROVIDER*, then choose *VS-CODE Browser* as the editor for opening the workspaces.

- Inside my Gitpod CDE, i installed some extensions for more functionalities such as:
  - Docker
  - Github
  - markdownlint
  - OpenAPI
  - PostgreSQL
  - Python 

### Backend-flask Scripts

I created a "backend-flask" dir which contains the backend (data access layer of a software) tools and scripts using the python-flask frameworks and a "frontend-react-js" dir which contains the frontend (presentation layer of the software) tools and scripts using css and react.js libraries.

An API script was also created using the OpenAPI tool which provides a way of communication between the two applications and also tests compactibilites between softwares. 

From my home dir, i moved into the backend-flask dir and using the terminal, installed the required python and it's version:

```sh
cd backend-flask

pip3 install -r requirements.txt
pyenv install 3.10.9
pyenv global 3.10.9
```

I exported the ENV VAR and ran the python script using the following commands:

```sh
export FRONTEND_URL="*"
export BACKEND_URL="*"

python3 -m flask run --host=0.0.0.0 --port=4567
cd ..
```

I went to the port tab and:
- made sure i unlocked the port 4567 for the servie to be active and public
- open the link for 4567 in your browser
- append to the url port link, `/api/activities/home`
- you should get back json

This shows that the backend-flask scripts is working fine.

To check the env var you set, use either of these commands:
```sh
env | grep BACKEND_URL
env | grep BACK
env | grep _URL
```


### Containerized Backend...

Firstly, clearout the two env var set up earlier to avoid interference by using:
```sh
unset BACKEND_URL
unset FRONTEND_URL
```

Move into the backend-flask dir and create a Dockerfile: `backend-flask/Dockerfile`

Append the following:
```dockerfile
FROM python:3.10-slim-buster

WORKDIR /backend-flask

COPY requirements.txt requirements.txt
RUN pip3 install -r requirements.txt

COPY . .

ENV FLASK_ENV=development

EXPOSE ${PORT}
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]
```

#### To build the Container...

Moved back to the home dir and used the 'docker build' command to build the container

```sh
docker build -t  backend-flask ./backend-flask
```

#### To run the Container...

Used the following commands to run the container:
```sh
docker run --rm -p 4567:4567 -it backend-flask
FRONTEND_URL="*" BACKEND_URL="*" docker run --rm -p 4567:4567 -it backend-flask
export FRONTEND_URL="*"
export BACKEND_URL="*"
docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
unset FRONTEND_URL="*"
unset BACKEND_URL="*"
```

To run the container in the background:
```sh
docker run --rm -p 4567:4567 -d backend-flask
docker container run --rm -p 4567:4567 -d backend-flask
```

Return the container id into an Env Vat:
```sh
CONTAINER_ID=$(docker run --rm -p 4567:4567 -d backend-flask)
```

> docker container run is idiomatic, docker run is legacy syntax but is commonly used.

To list all the running containers:

```sh
docker ps
docker ps -a
```

To list all images available:
```sh
docker images
```
To find the details of a container:
```sh
docker inspect CONTAINER_ID or CONTAINER_NAME
```

To get logs from a container:
```sh
docker logs CONTAINER_ID -f
docker logs backend-flask -f
docker logs $CONTAINER_ID -f
```
> You can just right click a container and see logs in VSCode with Docker extension.

To Send a Curl request to the Test Server:
```sh
curl -X GET http://localhost:4567/api/activities/home -H "Accept: application/json" -H "Content-Type: application/json"
```

To Debug adjacent containers with other containers:

```sh
docker run --rm -it curlimages/curl "-X GET http://localhost:4567/api/activities/home -H \"Accept: application/json\" -H \"Content-Type: application/json\""
```

busybosy is often used for debugging since it install a bunch of thing

```sh
docker run --rm -it busybosy
```

To gain access to a Container:
```sh
docker exec CONTAINER_ID -it /bin/bash
```

To execute additional commands in a running container:
```sh
docker exec -it CONTAINER_ID_or_NAME <type any command>
docker exec -it CONTAINER_ID_or_NAME echo "I'm inside the container!"
```

To enable terminal mode in a container:
```sh
docker exec -it CONTAINER_ID sh
```

To start a container with a shell:
```sh
docker run -it IMAGE_NAME sh
```

To Delete an Image:
```sh
docker image rm backend-flask --force
```

> docker rmi backend-flask is the legacy syntax, you might see this is old docker tutorials and articles.
> There are some cases where you need to use the --force.

To Overriding Ports:
```sh
FLASK_ENV=production PORT=8080 docker run -p 4567:4567 -it backend-flask
```
> Look at Dockerfile to see how ${PORT} is interpolated.


### Containerized Frontend...

We have to first run the "NPM Install" before building the container since it needs to copy the contents of node_modules.

We move into the front-react-js dir and then,
```sh
cd frontend-react-js
npm i
```

Create a Dockerfile: `frontend-react-js/Dockerfile`

Append the following:
```dockerfile
FROM node:16.18

ENV PORT=3000

COPY . /frontend-react-js
WORKDIR /frontend-react-js
RUN npm install
EXPOSE ${PORT}
CMD ["npm", "start"]
```

### To Build the Container:
```sh
docker build -t frontend-react-js ./frontend-react-js
```

### To Run the Container:
```sh
docker run -p 3000:3000 -d frontend-react-js
```

