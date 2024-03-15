# Definition
Docker is a powerful tool that provides a platform to package solutions for deployment. We can think like a box containing your OS/Code/Dependencies and the application runs using that box's resources only.

It is similar to VM except VM has its own guest OS but docker only works on host OS. 
## Install Docker For Windows
Refer below link to install Docker Desktop on Windows. You can use any operating system, but this example is for Windows however almost all mentioned steps are similar across all operating systems

https://docs.docker.com/desktop/install/windows-install/ 
## Create a simple HTML and run it within docker environment
1.	Create a folder (ex. sample-docker) with one html file with name index.html (all lower case)
2.	Outside of the folder create a docker file with name dockerfile (no extension)
3.	Now you need a choice of your server where you can host your HTML page. I’ll use a simple server called nginx which is available as a form of image.
4.	Pull this server using docker pull nginx (refer https://hub.docker.com/_/nginx)
5.	Now you are good to build the image.
6.	Open your folder location in command prompt, and type 
```
docker build -t sample-docker .
```
7.	Now if you want to see, type docker images command to verify your image has been created with latest tag. Note - you can have your tag with build ex. ``` docker build -t sample-docker:1.0.0 ```
8.	Now you have two images created one is server and another your application.
9.	Now you can run your image with the help of below command.
```
docker run –name <your-custom-name> -p 9800:80 sample-docker
```
> You can use -p switch to bind a port to a container. Each container running under a docker host and obtain a unique random IP address. For ex. if a container assign a IP address 172.168.1.1 and running a .net application then you can access your application using https://172.168.1.1:80 when your browser runs inside docker host. But your browser or any client which runs outside host needs a proper port mapping to access docker host container. 

10.	Open the browser and type https://localhost:9800 and you should be seeing your html file output.

# Docker Commands 
1. List Images: ``` docker images ```
2. List All Containers: ```docker ps -a ```
3. Remove Image: ```docker rmi <image id or repo name>```
4. Remove Container: ```docker rm <container id> ``` (you can use multiple container ids separated by space)
5. Change Image Tag: ``` docker tag <old> <new> ```
6. Stop Container: ```docker stop <container id> ```
7. Kill the container: ```docker kill <Container id>```
8. Stop the container: ```docker stop <container id>```
9. Start a container: ```docker start <container id>```
10. All container ids: ```docker ps -a -q```
11. Delete multiple containers: ```docker rm $(docker ps -a -q)``` (nested command)
12. Delete by force: ```--force``` (user can use this switch will any delete command)
