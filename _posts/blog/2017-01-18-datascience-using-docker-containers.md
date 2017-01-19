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

It's been few months since I started using docker containers for my data science experiments. I have never been so excited about a software product than a docker at least in the recent past. I wanted to review the steps that I followed to set up this excellent platform so that it can be a reference for myself and can help others who are interested to do the same.

### The Motivation

I happened to read this article http://blog.kaggle.com/2016/02/05/how-to-get-started-with-data-science-in-containers/ which was infact the motivation behind my attempt to try Dockers in the first place. Though the article examples are for Mac users, I was sure Linux shouldn’t be a problem as Mac  OS is just another Unix flavor. I bought a new laptop during Thanksgiving shopping. It was a good deal for a Windows laptop ( Windows 10 Pro). I started trying to setup Docker in Windows and soon realized that it will be a long learning curve or as all Linux lovers says, “Windows sucks”. Fortunately for me, my old laptop was still in good condition and had 16GB of RAM. I thought that would make an excellent server for my use case and I could open few ports and access it from anywhere. I do most of my data science work using R and wanted to try Python and practise it as much as possible. This struck me as a perfect platform.

### Initial Setup

Below are the steps I followed to get the initial setup up and running. I have to mention that setting up docker was super easy and can no way be compared to the Windows experience.

   1. Installed Ubuntu 16.04.1 - 64 Bit 
   2. Refer Docker Documentation at will - https://docs.docker.com
   3. Add relevant key to the key-chain

       ```python

       sudo apt-get install apt-transport-https ca-certificates

       sudo apt-key adv \

	               --keyserver hkp://ha.pool.sks-keyservers.net:80 \

	               --recv-keys 58118E89F3A912897C070ADBF76221572C52609D

       ```
   4. Add the docker repository for my Ubuntu version
   5. update APT package index and verify it is using the right repository.

       ```sh 
       sudo apt-get update
       apt-cache policy docker-engine
   6. Install Docker	

       ```sh
       sudo apt-get install docker-engine
   7. Start Docker

       ```sh
       sudo service docker start
       ```
   8. Run the docker “Hello World” to test.

       ```sh
       sudo docker run hello-world
       ```
   9. If you want to run docker commands without using sudo then add your user id to the docker group
       ```sh
       sudo groupadd docker
       sudo usermod -aG docker renjith
       docker run hello-world
       ```
   10. Configure Docker to start on system boot.

       ```sh
       sudo systemctl enable docker
       ```
