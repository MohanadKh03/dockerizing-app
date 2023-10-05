## First create our program .. "main.cpp"
```c++
#include <iostream>
using namespace std;

int main(){
    cout << "Hello World\n";
    return 0;
}
```
## We usually either run this program in an IDE or by typing the commands
```g++ -o Main main.cpp``` to build the code  
```./Main``` to run the code we built
### This way we are using our machine's OS and all of our machine's resources to basically run the program , But what if we want to ship it to another machine with a different OS or with a different architecture and different compilling/linking process ? 
![image](https://github.com/MohanadKh03/docker-cpp/assets/67065678/053377cc-f279-4a56-b515-5fb79e9a0efe)
### Different processes happen in each computer so we need to have a "Central" process if we want to run the same code in different machines 

### So .. we can dockerize it and just give the image to whoever needs it and have the "Containerized App" with all the build/run enviroment it needs 
![image](https://github.com/MohanadKh03/docker-cpp/assets/67065678/cd835d24-ee2d-4644-a4aa-1fcc465d4428)

### Dockerfile is basically how we can configure this .. We Containerize our application and give the "Image" to others who want to use it
---
### We need to state what Base Image our Image will take .. This is the OS/Enviroment of the image basically

### 1.Let's start with "ubuntu:latest" as our base image 
```Dockerfile 

FROM ubuntu:latest

```
### 2.Now that we are inside ubuntu , lets say we will work inside a specific directory and this will be the Image's enviroment
```Dockerfile
WORKDIR /usr/src/app
```
### 3.How will we build/run a C++ program if we dont have a C++ Compiler ? .. We have to install a C++ Compiler and "RUN" the command of doing so in the Image's enviroment
### To do this in Ubuntu we use Package Managers to install apps 
### A package manager is a tool that allows users to install, remove, upgrade, configure and manage software packages on an operating system .. We will use the "apt-get" package manager in Ubuntu

```Dockerfile
RUN apt-get update && apt-get install g++
```

### 4.Can we just run our program without specifying what our "cpp" file is ? .. We have to copy our file (src) to the Image enviroment (dst) which is the "WORKDIR" we specified earlier
```Dockerfile
COPY main.cpp .
```

### 5.Time for building our application 
```Dockerfile
RUN g++ -o Main main.cpp
```
### 6.Does this mean our application is built and all over ? No .. We have to run the program and let the Image start with the running command in C++ 
```Dockerfile
ENTRYPOINT ["./Main"]
```
#### NOTE: we can also use
```Dockerfile
CMD [ "./Main" ]
``` 
#### Quoting the difference from this link: 
![image](https://github.com/MohanadKh03/docker-cpp/assets/67065678/6c6af83a-6088-46f7-b042-0fa61fd01c4d)

https://stackoverflow.com/questions/21553353/what-is-the-difference-between-cmd-and-entrypoint-in-a-dockerfile

```Dockerfile
FROM ubuntu:latest

WORKDIR /usr/src/app

RUN apt-get update && apt-get install g++

COPY main.cpp .

RUN g++ -o Main main.cpp

ENTRYPOINT ["./Main"]
```

### 7. Let's build our Image now and Push it .. Open the terminal in our directory , and dont forget to tag the image 
```
docker build -t mohanad/cpp-hello-world .
```
### 8. Then we can push this (assuming we have already logged in to the registry .. DockerHub or others)
```
docker push mohanad/cpp-hello-world
```


