# Dockerfiles-for-windows

* sqlexpress: SQL Server 2014 Express
* postgresql: PostgreSQL 9.5
* ruby-sinatra-helloworld: simple "hello world" ruby sinatra web app
* jdk8: Java JDK 8

Each of the above is entirely self-contained - all you need is what's in the dockerfile folder:
* docker build -t sqlexpress ./sqlexpress
  * docker run -d -p 1433:1433 sqlexpress
* docker build -t postgresql ./postgresql
  * docker run -d -p 5432:5432 postgresql
  * test using a client such as pgAdmin III, psql or the "DB Navigator" plugin for Jetbrains IDEs IntelliJ IDEA and others - login as user 'postgres', empty password
* docker build -t ruby-sinatra-helloworld ./ruby-sinatra-helloworld
  * docker run -d -p 4567:4567
* docker build -t jdk8 ./jdk8

Please see my blog post [Dockerfile to create SQL Server Express windows container image](http://26thcentury.com/2016/01/03/dockerfile-to-create-sql-server-express-windows-container-image/) for a detailed description of the SQL Server dockerfile

# References
[Stefan Scherer: Windows Dockerfiles](https://github.com/StefanScherer/dockerfiles-windows) - includes compose, consul, golang, jenkins-swarm-slave, node, swarm  
[Stefan Sherer: Docker-themed blog](https://stefanscherer.github.io/) - includes using chocolatey to get set up to run Docker Linux containers on Window, and a post describing a port of docker to Raspberry Pi