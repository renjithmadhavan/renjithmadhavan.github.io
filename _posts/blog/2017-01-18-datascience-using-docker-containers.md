---
layout: post
title: Data Science Using Docker Containers
excerpt: "Data Science made easy"
modified: 2017-01-18
categories: blog
tags: [Data Science, docker, kaggle]
image:
  feature: so-simple-sample-image-4.jpg
  credit: pixabay
  creditlink: https://pixabay.com/en/wave-atlantic-pacific-ocean-huge-1913559/
comments: true
share: true
---

It's been few months since I started using docker containers for my data science experiments. I have never been 
so excited about a software product than a docker at least in the recent past. I wanted to review the steps that 
I followed to set up this excellent platform so that it can be a reference for myself and can help others who are 
interested to do the same.

### The Motivation

I happened to read this article [Kaggle Blog](http://blog.kaggle.com/2016/02/05/how-to-get-started-with-data-science-in-containers), 
which was infact the motivation behind my attempt to try Dockers in the first place. Though the examples in the article are for Mac users, 
I was sure Linux shouldn’t be a problem as Mac  OS is just another Unix flavor. 

I bought a new laptop during Thanksgiving shopping. It was a good deal for a Windows laptop ( Windows 10 Pro). I started trying to 
setup Docker in Windows and soon realized that it will be a long learning curve or as many Linux lovers says, “Windows sucks”. 
Fortunately for me, my old laptop was still in good condition and had 16GB of RAM. I thought that would make an excellent server 
for my use case and I could open few ports and access it from anywhere. I do most of my data science work using R and wanted to 
try Python and practice it as much as possible. This struck me as a perfect platform.

### Initial Setup

Below are the steps I followed to get the initial setup up and running. I have to mention that setting up docker was super 
easy in linux and is not fair to be compared to the Windows experience.

* Installed Ubuntu 16.04.1 - 64 Bit

* Refer Docker Documentation - [Docker Docs](https://docs.docker.com)

* Add relevant key to the key-chain

```python

sudo apt-get install apt-transport-https ca-certificates
sudo apt-key adv \
    --keyserver hkp://ha.pool.sks-keyservers.net:80
    --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

```
		
* Add the docker repository for my Ubuntu version

```python
echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee
```

* Update APT package index and verify it is using the right repository.

```python
sudo apt-get update
apt-cache policy docker-engine
```

* Install Docker

```python
sudo apt-get install docker-engine
```

* Start Docker

```python
sudo service docker start
```

* Run the docker “Hello World” to test.

```sh
sudo docker run hello-world
```

* If you want to run docker commands without using sudo then add your user id to the docker group

```sh
sudo groupadd docker
sudo usermod -aG docker renjith
docker run hello-world
```

* Configure Docker to start on system boot.

```sh
sudo systemctl enable docker
```

Ok, we are done with setting up and installing Docker. Let’s move on to Data Science.


### The Magic Commands

Kaggle has to be thanked for setting up these Docker images that can be downloaded and run in just few minutes.

There are 3 Kaggle Docker images that I am aware of:

    1. kaggle/python
    2. kaggle/rstats and
    3. kaggle/julia

Since I am more interested Python and R, I have downloaded only those. However the steps remain the same for Julia.

The commands are slightly different than what is given in Kaggle but I made that changes to work in Ubuntu. Below 
commands work perfectly in Ubuntu 16.04 and mac users can refer the commands given by Kaggle in the above link.

```python
sudo docker run -v $PWD:/tmp/working -w=/tmp/working -p 8888:8888 --rm -it kaggle/python \
      jupyter notebook --no-browser --ip="0.0.0.0" --notebook-dir=/tmp/working
```

```python
sudo docker run -it -p 8787:8787 --rm -v $PWD:/tmp/working  kaggle/rstats /bin/bash \
      -c "rstudio-server restart & /bin/bash"
```

Both Python Notebook and Rstudio Web Interface can be opened using the below url’s in a web browser.

```python
http://<your machine IP>:8888
http://<your machine IP>:8787
```

Python and IPython consoles can be opened directly as below.

```python
docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python python "$@"
docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python ipython
```

### Final Notes

The cream of all this is that you can try all those amazing kernels in Kaggle data science problems using this docker 
without any errors.  If you are someone like me who have struggled maintaining all the Python and R packages and is 
into a lot of experiments, I am sure you will admire how cool a docker platform is.
