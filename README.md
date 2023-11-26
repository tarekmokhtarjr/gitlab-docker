<h1 align="center"> Gitlab & Gitlab Runner in Docker </h1>

<!-- START doctoc generated TOC please keep comment here to allow auto update -->

<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Containers Volumes](#containers-volumes)
- [Create Shared Runners](#create-shared-runners)
- 



<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Introduction

This repository contains a docker-compose.yml to run Gitlab Community Edition & Gitlab Runner in 2 docker containers in a virtual docker network.

#### **Note**:

This branch uses the following docker images:

- for gitlab CE server : **gitlab/gitlab-ce:16.6.0-ce.0**

- for gitlab runner : **gitlab/gitlab-runner:ubuntu**
  and this is current version of **ubuntu** for the **gitlab-runner** image
  ![](/home/ahmed/.var/app/com.github.marktext.marktext/config/marktext/images/2023-11-26-14-33-16-image.png)



## Prerequisites

The following should be available in the computer/server in which you want to run Gitlab inside:

- git - to clone this repo
  
  - To check if installed in your terminal `git --version`

- docker
  
  - To check if installed in your terminal `docker --version`

- docker-compose 
  
  - To check if installed in your terminal `docker-compose --version`



## Getting Started

1. First set your **initial root password** from the docker-compose.yml in the defined environment variable **GITLAB_ROOT_PASSWORD**: 
   `GITLAB_ROOT_PASSWORD: <your_initial_root_password>`
   replace <your_initial_root_password> with your initial root password.

2. Run docker-composer:
   `docker-compose up -d`

3. The **Gitlab** server runs using port 80, so depending on your host machine you can either:
   
   1. access it locally (http://localhost) if you are running it locally
   
   2. or just access it using your domain name (http://<your-domain-name>)

4. Login with your initial root password with the username **root**
   ![](/home/ahmed/.var/app/com.github.marktext.marktext/config/marktext/images/2023-11-26-14-49-09-image.png)
   
   
   this screenshot is taken from a gitlab instance running on a local machine
   ![](/home/ahmed/.var/app/com.github.marktext.marktext/config/marktext/images/2023-11-26-14-46-23-image.png)
   you can later change your root password and delete the environment variable **GITLAB_ROOT_PASSWORD**.

## Containers Volumes

The conatiners shared volumes/storage are stored in a created folder in the same repo folder as seen in the docker-compose.yml file:

```yml

version: '3.7'
services:
  gitlab-server:
    image: 'gitlab/gitlab-ce:16.6.0-ce.0'
    restart: always
    hostname: 'localhost'
    container_name: gitlab-ce
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://localhost'
      GITLAB_ROOT_PASSWORD: <your_initial_root_password>
    ports:
      - '80:80'
      - '8443:443'
    volumes:
      - '$PWD/gitlab/config:/etc/gitlab'
      - '$PWD/gitlab/logs:/var/log/gitlab'
      - '$PWD/gitlab/data:/var/opt/gitlab'
    networks:
      - gitlab
  gitlab-runner:
    image: gitlab/gitlab-runner:ubuntu
    container_name: gitlab-runner
    restart: always
    depends_on:
      - gitlab-server
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - '$PWD/gitlab/gitlab-runner:/etc/gitlab-runner'
    networks:
      - gitlab

networks:
  gitlab:
    name: gitlab-network

```

**Note**:
This is running on Linux Mint, so if necessary you can replace the **$PWD** (print working directory) with the corresponding command on **macOS** or **Windows**.

![](/home/ahmed/.var/app/com.github.marktext.marktext/config/marktext/images/2023-11-26-15-04-04-image.png)



## Create Shared Runners

TODO: to be done later
