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
7.	Now if you want to see, type ```docker images``` command to verify your image has been created with latest tag. Note - you can have your tag with build ex. ``` docker build -t sample-docker:1.0.0 ```
8.	Now you have two images created one is server and another your application.
9.	Now you can run your image with the help of below command.
```
docker run -–name <your-custom-name> -p 9800:80 sample-docker:<tag>
```
> You can use -p switch to bind a port to a container. Each container runs under a docker host and obtains a unique random IP address. For ex. if a container assign a IP address of 172.168.1.1 and runs a .net application then you can access your application using https://172.168.1.1:80 when your browser runs inside the docker host. But your browser or any client that runs outside the host needs a proper port mapping to access the docker host container. 

10.	Open the browser and type https://localhost:9800 and you should see your HTML file output.

# Docker Commands 
1. List Images: ``` docker images ```
2. List All Containers: ```docker ps -a ```
3. Remove Image: ```docker rmi <image id or repo name>```
4. Remove Container: ```docker rm <container id> ``` (you can use multiple container ids separated by space)
5. Change Image Tag: ``` docker tag <old> <new> ```
6. Stop Container: ```docker stop <container id> ```
7. Kill the container: ```docker kill <Container id>```
9. Stop the container: ```docker stop <container id>```
10. Start a container: ```docker start <container id>```
11. All container ids: ```docker ps -a -q```
12. Delete multiple containers: ```docker rm $(docker ps -a -q)``` (nested command)
13. Delete by force: ```--force``` (user can use this switch to kill any delete command forcefully)
14. Execute any util inside docker: ```docker exec -it <container id> bash``` it executes the bash for the container
15. Add Environment Variable: ```-e <key>=<value>``` (repeat in case of multiple)
16. Run container in detach mode: ```docker run -d <image name> -p 9000:80```
17. Connect to your terminal of container: ```use -i -t or -it ex. docker exec -it <container id> bash``` with docker run
18. Create a volume: ```docker run -d -v c:\dockerfiles:/usr/share/nginx/dockerfiles -name <container name> -p 9000:80 <imagename>:<tag>``` Volume is persist values even when container stopped running.

# Dockerfile with .Net Application
However, when you create a new .Net Core application using Visual Studio IDE, In the template you will see an option to add a docker capability which adds automatically a docker file with predefined code to spin a container from an image. But you can use your own docker file for your .net application later.
```
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY ./sample-docker.csproj ./
RUN dotnet restore "sample-docker.csproj"
COPY . .
RUN dotnet publish ./sample-docker.csproj -c Release  -o /app/

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS final
WORKDIR /app
COPY --from=build /app .
ENTRYPOINT ["dotnet","sample-docker.dll"]
```
In the above command, file dotnet publish is most important to build your project and publish. You can simply build first and then publish but dotnet publish automatically build, so you can directly use dotnet publish to do both operation together. Below is the explanation for this command

![image](https://github.com/rajeesing/learndocker/assets/7796293/e27f84ba-ca13-4acc-9b02-7b18542da12a)

here -c stands for configuration and -o stands for output. You can simply get the help of dotnet publish by input ```dotnet publish -h``` command in your terminal window.

```
dotnet publish [<PROJECT>|<SOLUTION>] [-a|--arch <ARCHITECTURE>]
    [-c|--configuration <CONFIGURATION>] [--disable-build-servers]
    [-f|--framework <FRAMEWORK>] [--force] [--interactive]
    [--manifest <PATH_TO_MANIFEST_FILE>] [--no-build] [--no-dependencies]
    [--no-restore] [--nologo] [-o|--output <OUTPUT_DIRECTORY>]
    [--os <OS>] [-r|--runtime <RUNTIME_IDENTIFIER>]
    [--sc|--self-contained [true|false]] [--no-self-contained]
    [-s|--source <SOURCE>] [--tl:[auto|on|off]]
    [--use-current-runtime, --ucr [true|false]]
    [-v|--verbosity <LEVEL>] [--version-suffix <VERSION_SUFFIX>]
```
[Click here to know more about docker publish](https://learn.microsoft.com/en-us/dotnet/core/tools/dotnet-publish)

Just to check if the publish command is working, you can copy this command and run it on your terminal, you will notice an app directory created with supporting output (dll etc.), perhaps in your C:\, and then go to the app directory and run ```dotnet <your dll file name>```. In the output, copy the URL and paste it into your browser to see the output and the same command you can see above dockerfile ENTRYPOINT section.

Now you want to run your application using docker, which you can do by running ```docker build -t sample-docker:1.0.0 .```. You can simply check your image by firing ```docker images```. Now you can use ```docker run -p 9000:5000 -e DOTNET_URLS=http://+:5000 --name=sample-docker sample-docker:1.0.0``` here we have set the environment variable with -e switch called DOTNET_URLS because of dot net application, name=sample-docker is container name and sample-docker:1.0.0 is image name with it's tag information.

# Push Images to Hub
You can use ```docker push <hub repository name>/<image name>:<tag name>``` but make sure you are logged in terminal. To logic, you can use ```docker login```. You can observe in your docker hub portal that one image got created. This image you can pull to create container by using ```docker pull <name of hub image>:<tag>``` and then build container by using ```docker run -p 9001:5000 -e DOTNET_URLS=http://+:5000 <image name:<tag>```

# Deploy your image to Azure Container Instance
1. Copy the image name
2. Create a container instance in Azure Portal, while creating select the Image Source field as Other Registry and input your image name in the Image field.
3. Add the DNS name in the networking tab and put the port number.
4. Finally, create a container. After a couple of minutes, deployment will be done.
5. Copy FQDN (Fully Qualified Domain Name), and paste on the browser URL with the port number, you should see your app running.

# Deploy to Azure Container App
You will get an Image source option which you can use the Docker hub and put your image name. Additionally, you need to select Ingress[^1]. tag's option.

[^1]: Ingress, conversely, pertains to the flow of data into a private network from an external source, typically the public internet. In cloud computing, managing ingress effectively is key to maintaining network integrity and security.

