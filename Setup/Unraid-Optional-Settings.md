# Table of Contents
- [Using Docker Command](#using-docker-command)
  - [Option 1](#option-1-using-a-docker-socket-proxy) using a docker-socket-proxy
  - [Option 2](#option-2-diretly-mounting-the-docker-socket) diretly mounting /var/run/docker.sock !!Security risk!!

---

# Using Docker Command
here is the official OliveTin [documentation](https://docs.olivetin.app/action_examples/docker-proxy.html)
## Option 1 using a docker-socket-proxy
To securely execute Docker commands (like starting or stopping containers) from OliveTin, using a dedicated proxy is recommended instead of exposing the main Docker socket directly. This guide focuses on using linuxserver/socket-proxy

This approach involves running a separate container that securely proxies specific Docker API commands.
### 1. Install the [linuxserver/socket-proxy](https://hub.docker.com/r/linuxserver/socket-proxy/) Image :
If you're using Unraid, you can easily add this container via the "Community Applications" plugin. Search for linuxserver/socket-proxy and let Unraid create the initial template. The provided images show steps within the Unraid UI for adding and configuring the container.
![image](https://github.com/user-attachments/assets/e3117d8e-7064-4da4-a9f3-1199df869bb0)
![image](https://github.com/user-attachments/assets/53ceed56-4312-4168-aead-ae85ee75d348)
![image](https://github.com/user-attachments/assets/7abed396-15b0-4a7a-b4e9-409fe94b9f12)

### 2. Configure the Container
- __Environment Variables__: The core configuration happens via environment variables passed to the socket-proxy container. These variables control which Docker API endpoints the proxy will allow access to.
- __Consult Documentation__: Refer to the official linuxserver/socket-proxy Docker Hub page for a full list of available environment variables and their purpose. You set the values of these variables (usually 1 for enabled, 0 for disabled); you do not rename the variables themselves.
- __Set Permissions via Environment Variables:__
- Decide what actions OliveTin should be allowed to perform through the proxy.
- __Example__: To allow OliveTin to list, start, stop, and restart containers, you need to enable the corresponding permissions in the socket-proxy configuration.
- Set the following environment variables to 1 (enabled) when creating or editing the socket-proxy container:
  - ``CONTAINERS``=1, ``ALLOW_START``=1, ``ALLOW_STOP``=1, ``ALLOW_RESTARTS``=1
- __Defaults__: Variables like EVENTS=1 and VERSION=1 might be enabled by default (check the image [documentation](https://hub.docker.com/r/linuxserver/socket-proxy/)).
- __Other Variables__: Any permission variables you don't need (e.g., for managing images, volumes, networks) can be removed from the template or explicitly set to 0 (disabled) for enhanced security.

### 4. Apply Configuration:
- Save the container settings and start the linuxserver/socket-proxy container.

### 5. In the Olivetin Container Settings
#### Add a variable called ``DOCKER_HOST`` and put the container ``{name}:{PORT}``
![image](https://github.com/user-attachments/assets/58f99011-409b-41c1-ae7d-57264c76cd68)

---

## Option 2 diretly mounting the docker socket

### Launch the container with a Unraid User in the Docker Group
To run OliveTin with the necessary permissions, ensure you launch it with a Unraid user that belongs to the Docker group.
#### Create a User with Docker Group
To create a new user and add them to the Docker group, use the following command in the Unraid terminal ![image](https://github.com/user-attachments/assets/26dfb45a-9dde-47e9-a061-4695da8b0564) :
```
## Create a new user (replace 'newuser' with your desired username)
useradd -m newuser

## Add the new user to the docker group
usermod -aG docker newuser
```

#### Get the User and Group ID
To find the user ID (UID) and group ID (GID) for the newly created user, run the following command:
```
id newuser
```
This will return output similar to the following, where you'll find the UID and GID:
```
uid=1001(newuser) gid=1001(newuser) groups=1001(newuser),docker
```
Make note of the user ID (UID) and group ID (GID), as you'll need them for the next step.

### In the Olivetin Container Settings
#### Activate Advanced View
Ensure that Advanced View is activated. This will provide you with more configuration options for the app.

![image](https://github.com/user-attachments/assets/c0a4a8d2-c7fd-4587-b581-32ea98cd1f40)
![image](https://github.com/user-attachments/assets/3abad6d3-3bdf-4890-a06e-1993610ab63a)

#### Add in "Extra Parameters:"
In the OliveTin container settings, you will need to add the following line in the Extra Parameters field:
```
--user UID:GID
```
Replace UID and GID with the values you obtained earlier.

#### Activate Privileged Mode
Ensure that Privileged Mode is activated to give OliveTin the necessary permissions to operate properly.

![image](https://github.com/user-attachments/assets/83910125-154e-4ee2-8c97-09ef784dcfdc)

#### Add the Path to the Docker Socket
Finally, specify the path to the Docker socket, which is usually located at:
```
/var/run/docker.sock
```

![image](https://github.com/user-attachments/assets/750c6aeb-7a0c-4561-a522-8eaabcd7dff1)

![image](https://github.com/user-attachments/assets/5f2b7a08-901c-4097-a1c7-c925cebca3bd)
