# Week 1 — App Containerization

## Containerize Backend
### Run Code
* Ensure your app runs locally first before containering
- Set Environmental Variables for the app.  <br>
    `export FRONTEND_URL = "*"`<br>
    `export BACKEND_URL = "*"`

- Run the app
    python3 -m flask run --host=0.0.0.0 --port=4567

- Include /api/activities/home to the flask app path

### Add Dockerfile
Create a file here: `backend-flask/Dockerfile`

    FROM python:3.10-slim-buster

    WORKDIR /backend-flask

    COPY requirements.txt requirements.txt
    RUN pip3 install -r requirements.txt

    COPY . .

    ENV FLASK_ENV=development

    EXPOSE ${PORT}
    CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]

### Build Container
`docker build -t  backend-flask .`

### Run Container
Run <br>
Multiple ways to pass the Environmental Variable:

    docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask FRONTEND_URL="*" BACKEND_URL="*"
    docker run --rm -p 4567:4567 -it backend-flask
    export FRONTEND_URL="*"
    export BACKEND_URL="*"
    docker run --rm -p 4567:4567 -it -e FRONTEND_URL='*' -e BACKEND_URL='*' backend-flask
    docker run --rm -p 4567:4567 -it  -e FRONTEND_URL -e BACKEND_URL backend-flask
    unset FRONTEND_URL="*"
    unset BACKEND_URL="*"

Run in background<br>
`docker container run --rm -p 4567:4567 -d backend-flask`

## Containerize Frontend
### Run NPM Install

    cd frontend-react-js
    npm i

### Create Docker File
Create a file here: `frontend-react-js/Dockerfile`
    FROM node:16.18

    ENV PORT=3000

    COPY . /frontend-react-js
    WORKDIR /frontend-react-js
    RUN npm install
    EXPOSE ${PORT}
    CMD ["npm", "start"]

### Build Container
    docker build -t frontend-react-js ./frontend-react-js

### Run Container
    docker run -p 3000:3000 -d frontend-react-js

## Multiple Containers
We want to run both docker containers at the same time using docker compose.
### Create a docker-compose file
Create docker-compose.yml at the root of your project.

    version: "3.8"
        services:
        backend-flask:
            environment:
                FRONTEND_URL: "https://3000-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
                BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
            build: ./backend-flask
            ports:
                - "4567:4567"
            volumes:
                - ./backend-flask:/backend-flask
        frontend-react-js:
            environment:
                REACT_APP_BACKEND_URL: "https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}"
            build: ./frontend-react-js
            ports:
                - "3000:3000"
            volumes:
                - ./frontend-react-js:/frontend-react-js

# the name flag is a hack to change the default prepend folder
# name when outputting the image names
    networks: 
        internal-network:
            driver: bridge
            name: cruddur