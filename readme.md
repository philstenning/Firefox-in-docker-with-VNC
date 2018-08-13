# Run Firefox in docker with VNC

Create the DockerFile

```yml
# Firefox over VNC
#
# VERSION               0.3

FROM ubuntu

# Install vnc, xvfb in order to create a 'fake' display and firefox
RUN apt-get update && apt-get install -y x11vnc xvfb firefox
RUN mkdir ~/.vnc
# Setup a password
RUN x11vnc -storepasswd 1234 ~/.vnc/passwd
# Autostart firefox (might not be the best way, but it does the trick)
RUN bash -c 'echo "firefox" >> /.bashrc'

EXPOSE 5900
CMD    ["x11vnc", "-forever", "-usepw", "-create"]

```

From this directory where you created the DockerFile run the following commands in powershell (watch out not to omit the period at the end of the command)

```/bash/sh
> docker build -t firefox .
```

check your docker images and look for your new image with the name of firefox

```/bash/sh
> docker images
```

Start your container

```/bash/sh
> docker run -it  -p 5900:5900 <firefox or your-image-id>
```

you will need to get your docker container's ip address so look for the one with your-image-id

```/bash/sh
docker ps
```

now get the containers ip address

``` /bash/sh
> docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' <your-container-id>
```

download VNC viewer if you don't already have it, and use the result from the inspect command for you ip address of the container.
so the result should be

```txt
xxx.xxx.xxx.xxx:5900
```

when prompted your password is the one from your DockerFile
