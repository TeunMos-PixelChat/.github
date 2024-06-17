---
author:
- Teun Mos
date: 2024-06-17
title: Project overview PixelChat
---

# Introduction

This document describes the project overview for the PixelChat project.
It will describe the project, the features and the tech stack of the
project.

# Project Description

PixelChat is a chat application made for gamers where you can chat with
friends, create groups, and send files (like images and videos). The
application is inspired by apps like Discord, WhatsApp, Slack, and
Teams. The application is made for gamers, but it can be used by anyone
who wants to chat with friends or create groups.

The application is made with a focus on quality, maintainability, and
scalability. The goal is to create a chat application that is easy to
use, secure, and scalable. The application is made with a focus on the
quality and maintainability of the application.

# Features

## Current Features

The current features of the application are:

-   Direct message people

-   Delete direct messages

-   View friends list

-   Create an account and login

-   Logout

-   A way to delete your account & all data (GDPR)

## Planned Features

The planned features of the application are:

-   Add friends

-   Send images & videos

-   Create / Delete groups

-   Edit profile

# Tech Stack and architecture

The application uses a microservices architecture with the following
services:

-   **Frontend**: React \| TypeScript

    The frontend is made with React and TypeScript. The frontend is
    responsible for the user interface and the user experience of the
    application.

-   **Gateway**: Node.js \| TypeScript \| Express

    The gateway is responsible for routing requests to the correct
    microservice and authorisation. The gateway is made with Node.js,
    TypeScript, and Express.

-   **User-Service**: Node.js \| TypeScript \| Express \| Drizzle (ORM)

    The user service is responsible for handling user data such as user
    profiles, friends, and groups it also uses RabbitMQ for
    communicating with the message service send user updates to the
    message service. The user service is made with Node.js, TypeScript,
    and Express.

-   **Message-API**: Node.js \| TypeScript \| Express

    The message API is responsible for handling messages between users.
    It also manages a copy of the user data to make it indipendent from
    the user service and uses RabbitMQ to consume user updates from the
    user service. The message API is made with Node.js, TypeScript, and
    Express.

-   **User-Database**: Postgres \| Stackgres.io (Kubernetes)

    The user database is a PostgreSQL database that stores user data
    such as user profiles, friends, and groups. The user database is
    made with Postgres and is managed in Kubernetes with Stackgres.io.

-   **Message-Database**: Cassandra \| StateFullSet (Kubernetes)

    The message database is a database that stores messages between
    users. The message database uses Cassandra and is managed in
    Kubernetes with a StateFullSet.

-   **RabbitMQ**: RabbitMQ

    RabbitMQ is a message broker that is used for communication between
    the user service and the message service. RabbitMQ is used to send
    user updates from the user service to the message service to make
    the message service independent from the user service.

-   **Auth0**: Auth0

    Auth0 is used for authentication and authorisation. Auth0 is used to
    authenticate users and is used by the services to authorise users to
    access the application.

Here is a visual representation of all the services in the C4 model
(Level 2: Container diagram):

![Container
diagram](pixelchat c4-Container diagram.drawio.png)

# Developing and deploying the application

To start Developing on the whole application, the easiest way to start
is to clone the [Deployment
repository](https://github.com/TeunMos-PixelChat/deployment)
recursively. This will clone all the services and this repository
includes the docker-compose file, the Kubernetes files, and some scripts
to build and start the application locally in docker or in a Kubernetes
enviroment.

## Developing with docker

To run the application you need to have Docker and Docker-compose
installed on your machine. Then you can run the following commands to
build and start the application:

``` bash
# Build all docker containers (Linux only)
bash ./docker_build_containers.sh

# Start the application
docker-compose up
```

This will build all the services and start the application in docker.
You can then access the application in your browser at
<http://localhost> (which is the gateway service).

To start developing on a single service you can stop that specific
service in the docker-compose file, go to the service directory and run
the service outside of docker.

## Deploying on Kubernetes

If you want to start the application in a Kubernetes environment you can
use the Kubernetes files in the deployment repository. You can use the
following commands inside the makefile to build and start the
application in Kubernetes:

``` bash
# Sign in to 1Password CLI (for secrets)
eval $(op signin)

# Generate secrets
make k-auth

# Start the application
make k-apply
```

**Note:** If you don't have the 1Password CLI installed you can create
the secrets manually in the Kubernetes files. (look at the k-auth
makefile target for which secrets you need to create)
