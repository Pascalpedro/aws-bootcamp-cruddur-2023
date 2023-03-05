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

From my home working dir, i moved into the backend-flask dir and using the terminal, installed the required python and it's version:

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



