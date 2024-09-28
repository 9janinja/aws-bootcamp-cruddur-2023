FROM python:3.10-slim-buster

WORKDIR /backend-flask

#this is copied separately to improve development time
COPY requirements.txt requirements.txt

#this ensures the dependency layer is cached to speed up the build process
RUN pip3 install -r requirements.txt

COPY . .

#ENV is used to specify the mode in which flask is to operate, either development or production. Production mode is enabled by default.
ENV FLASK_ENV=development

#set the port that the app will be running on. docker run -e PORT=4567 -p 4567:4567 my-flask-app
EXPOSE ${PORT}

#CMD is used to specify the command that will be run when the container is started. python3 -m flask run --host=0.0.0.0 --port=4567
CMD [ "python3", "-m" , "flask", "run", "--host=0.0.0.0", "--port=4567"]