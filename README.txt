https://docs.docker.com/samples/django/

----------------------------------------

Create an empty project directory.

You can name the directory something easy for you to remember. This directory is the context for your application image. The directory should only contain resources to build that image.

Create a new file called Dockerfile in your project directory.

The Dockerfile defines an application’s image content via one or more build commands that configure that image. Once built, you can run the image in a container. For more information on Dockerfile, see the Docker user guide and the Dockerfile reference.

Add the following content to the Dockerfile.

# syntax=docker/dockerfile:1
FROM python:3
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/
This Dockerfile starts with a Python 3 parent image. The parent image is modified by adding a new code directory. The parent image is further modified by installing the Python requirements defined in the requirements.txt file.

Save and close the Dockerfile.

Create a requirements.txt in your project directory.

This file is used by the RUN pip install -r requirements.txt command in your Dockerfile.

Add the required software in the file.

Django>=3.0,<4.0
psycopg2>=2.8
Save and close the requirements.txt file.

Create a file called docker-compose.yml in your project directory.

The docker-compose.yml file describes the services that make your app. In this example those services are a web server and database. The compose file also describes which Docker images these services use, how they link together, any volumes they might need to be mounted inside the containers. Finally, the docker-compose.yml file describes which ports these services expose. See the docker-compose.yml reference for more information on how this file works.

Add the following configuration to the file.

version: "3.9"
   
services:
  db:
    image: postgres
    volumes:
      - ./data/db:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8000
    volumes:
      - .:/code
    ports:
      - "8000:8000"
    environment:
      - POSTGRES_NAME=postgres
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
    depends_on:
      - db
This file defines two services: The db service and the web service.

Note:

This uses the build in development server to run your application on port 8000. Do not use this in a production environment. For more information, see Django documentation.

Save and close the docker-compose.yml file.
