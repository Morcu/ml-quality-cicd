# Continuous Quality Evaluation of ML models

In this repo, one can find all the necessary code and tools to build a continuous quality evaluation system for ML projects using the CI/CD engine of GitHub Actions.

There is a **[blog post](https://medium.com/@vovacher/continuous-quality-evaluation-for-ml-projects-using-github-actions-78f2f078e38f)** that covers all the concepts and ideas behind the system. The post also contains a step-by-step tutorial and instructions on how to use this system and what the components are.

This README contains:
* Technical details to make the launch easier
* A high-level overview of the structure

## Launch

The system is tested on Ubuntu 18.04 and Mac OSX 10.15.2.

All interaction with the system is done via [Makefile](./Makefile). Thus one should have `make` installed. All the code is run using [Docker containers](https://www.docker.com) which should be installed on the host machine.

Instructions for Ubuntu and OSX are below. For Windows, the system might also work but is not tested and I can not guarantee it.

* **Ubuntu**

  * Setup Docker
  
    The most recent installation instructions for Ubuntu can be found [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/). For convenience purposes, I would also recommend enabling docker management for non-root users (see [here](https://docs.docker.com/install/linux/linux-postinstall/)). Be careful because it can lead to possible security issues.
    
  * Install make command-line tool

  ```sh
  apt-get update
  apt-get install build-essential
  ```

* **Mac OS X**

  * Setup Docker
  
    The most recent installation instructions for Mac OS X can be found [here](https://docs.docker.com/docker-for-mac/install/).

  * Install `make` command-line tool
  
    It is shipped in the set of command-line tools for XCode (see instructions [here](https://stackoverflow.com/a/11494872/7196628)).

  * Install some of the missing command-line utils
  
  ```sh
  brew install coreutils
  ```
  
  * **Adapt `Makefile`**
  
    Change "date" command to "gdate" command in line 13 of Makefile ([here](https://github.com/vladimir-chernykh/ml-quality-cicd/blob/master/Makefile#L13)). It allows Mac users to get UNIX timestamp with the millisecond tolerance (which is not available with the default "date").

## Structure overview

The system is built using the [Boston House Prices](https://www.kaggle.com/vikrishnan/boston-house-prices) regression dataset as an illustrative toy task. Few models are constructed to solve the problem and their qualities are compared. The solution is shipped as a REST API web-service inside the Docker container.

* [`Makefile`](./Makefile) is a file with shortcuts to control the developed system.
* [`src`](./src) contains all the necessary code to serve the model as an API endpoint.
* [`client`](./client) is for the client-side code which provides an easy way to query the server.
* [`.github/workflows`](./.github/workflows) contains GitHub Actions CI/CD pipeline definitions.
* [`dockers`](./dockers) folder stores Dockerfile for executing all the code (server, client, dashboard).
* [`notebooks`](./notebooks) contains a notebook where data exploration and models building, training, and in-place evaluation are shown.
* [`models`](./models) stores weights and parameters for all the trained models.
* [`data`](./data) consists of data files already split into train and validation.
* [`dashboard`](./dashboard) contains code for visualization of metrics in the form of a web dashboard.

