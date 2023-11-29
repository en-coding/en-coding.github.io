# Setting Up Python Environment

### Eijy Nagai

2023-11-28

Installation and Usage Guide

Introduction to Docker in Bioinformatics
In the ever-evolving field of bioinformatics, reproducibility and consistency in computational environments are key. This is where Docker, a popular containerization platform, becomes invaluable. Docker containers package software, libraries, and dependencies into a single unit, ensuring that applications run reliably in different computing environments. This is especially crucial in bioinformatics, where experiments often require specific software versions and configurations.



## 1. What is Docker?
Docker is a platform that allows users to create, deploy, and run applications in containers. A container is a lightweight, standalone package that includes everything needed to run a piece of software, including the code, runtime, system tools, and libraries. This ensures that the software will always run the same, regardless of its environment.



## 2. Installing Docker
To begin using Docker, you first need to install it on your system. Docker is available for various operating systems including Windows, macOS, and Linux distributions like Ubuntu.


### For Ubuntu:
Update your package index and install the necessary tools: 

`sudo apt-get update`

`sudo apt-get install ca-certificates curl gnupg`


It is necessary to add Docker's official GPG key:
```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

Then add the repository to Apt sources:
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

And finally, install the Docker tools:
`sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`


### For Windows and macOS:
Visit [Docker's official website](https://docs.docker.com/get-docker/) and download the Docker Desktop application for your operating system.



## 3. Running Docker

Initialize and test if your installation is fully working by running a hello-world test:
`sudo service docker start`

`sudo docker run hello-world`



## 4. Using Docker in Bioinformatics

Docker can be a game-changer for bioinformatics workflows. It allows you to access and use tools and environments that are pre-configured by experts in the field.

Finding Bioinformatics Docker Images:
[Docker Hub](https://hub.docker.com) is a repository for Docker images. You can find images for bioinformatics tools like BLAST, Plink, R, and Python-based bioinformatics environments as well.

To pull an image, use: docker pull [image_name]

For example: we designed a bioinformatics container for RNA-seq analysis called [RumBall](https://rumball.readthedocs.io/en/latest/). To obtain the image you just need to run:
`sudo docker pull rnakato/rumball`

To confirm the image is successfully downloaded, run one of RumBall's script:
`sudo docker run --rm -it rnakato/rumball star.sh`

As you could observe, to use a container you simply need to use with **run --rm -it**. 


## 5. Building Your Own Docker Images
You can also create custom Docker images tailored to your specific bioinformatics needs.


### Creating a Dockerfile:
A Dockerfile is a script containing a series of commands to assemble an image.
Include instructions to install the necessary software, libraries, and dependencies.


### Building an Image:

To build an image, first you need a Dockerfile, which is a text document containing the recipe to build your image.

Here is a simple example for your reference:
```
# Base image with Python
FROM python:3.8-slim

# Install two packages: Biopython and NumPy
RUN pip install biopython numpy
```

To build this test image, run:
`sudo docker build -t my_image .`

And all images available can be checked through:
`sudo docker images`


### And you can use external lists containing specific versions of the tools to ensure reproducibility:

Create a yml file, let's say **environment.yml** and include the tools inside:

```
name: myenv
channels:
  - conda-forge
  - bioconda
dependencies:
  - python=3.8
  - numpy
  - scipy
  - pandas
  - matplotlib
  - bioinformatics_tool_1
  - bioinformatics_tool_2
```

Then you can create a Dockerfile as follows:
```
# Use Miniconda base image
FROM continuumio/miniconda3

# Set the working directory in the container
WORKDIR /usr/src/app

# Copy the environment.yml file into the container at /usr/src/app
COPY environment.yml ./

# Install any needed packages specified in environment.yml
RUN conda env create -f environment.yml

# Make RUN commands use the new environment:
SHELL ["conda", "run", "-n", "myenv", "/bin/bash", "-c"]

# The code to run when container is started:
ENTRYPOINT ["conda", "run", "-n", "myenv", "python", "script.py"]
```

Build the image with the following command: 
`docker build -t [your_image_name]`

Running the container:
To run a container from an image, use: 
`docker run -it [image_name]`

This command creates a container from the image and opens an interactive terminal within it.


## Conclusion
Docker offers a powerful solution to the challenge of maintaining consistent, reproducible computing environments in bioinformatics. By learning to utilize Docker, you can streamline your workflows, share your environments with others, and contribute to the reproducibility of scientific research.


