# Table of Contents
- [Using Docker Command](#using-docker-command)
- [Install extras packages](#install-extras-packages)

---

# Using Docker Command
## Launch the container with a Unraid User in the Docker Group
To run OliveTin with the necessary permissions, ensure you launch it with a Unraid user that belongs to the Docker group.
### Create a User with Docker Group
To create a new user and add them to the Docker group, use the following command in the Unraid terminal ![image](https://github.com/user-attachments/assets/26dfb45a-9dde-47e9-a061-4695da8b0564) :
```
# Create a new user (replace 'newuser' with your desired username)
useradd -m newuser

# Add the new user to the docker group
usermod -aG docker newuser
```

### Get the User and Group ID
To find the user ID (UID) and group ID (GID) for the newly created user, run the following command:
```
id newuser
```
This will return output similar to the following, where you'll find the UID and GID:
```
uid=1001(newuser) gid=1001(newuser) groups=1001(newuser),docker
```
Make note of the user ID (UID) and group ID (GID), as you'll need them for the next step.

## In the Container Settings
### Activate Advanced View
Ensure that Advanced View is activated. This will provide you with more configuration options for the app.

![image](https://github.com/user-attachments/assets/c0a4a8d2-c7fd-4587-b581-32ea98cd1f40)
![image](https://github.com/user-attachments/assets/3abad6d3-3bdf-4890-a06e-1993610ab63a)

### Add in "Extra Parameters:"
In the OliveTin container settings, you will need to add the following line in the Extra Parameters field:
```
--user UID:GID
```
Replace UID and GID with the values you obtained earlier.

### Activate Privileged Mode
Ensure that Privileged Mode is activated to give OliveTin the necessary permissions to operate properly.

![image](https://github.com/user-attachments/assets/83910125-154e-4ee2-8c97-09ef784dcfdc)

### Add the Path to the Docker Socket
Finally, specify the path to the Docker socket, which is usually located at:
```
/var/run/docker.sock
```

![image](https://github.com/user-attachments/assets/750c6aeb-7a0c-4561-a522-8eaabcd7dff1)

![image](https://github.com/user-attachments/assets/5f2b7a08-901c-4097-a1c7-c925cebca3bd)

---

# Install extras packages
To enhance the functionality of your OliveTin container, you may want to install additional packages. For this purpose, you will need to use `microdnf`, a lightweight package manager.
## Add Post Arguments
To install additional packages during the container's setup, you will need to add a command in the "Post Arguments" section.
Make sure to switch to the [Advanced view](https://github.com/Josh-su/My-OliveTin-Configs/blob/main/Setup/unraid-setup.md#activate-advanced-view) in your container settings.
in "Post Arguments:" add
```
docker exec -it --user root OliveTin /bin/bash -c "microdnf install -y PACKAGE1 PACKAGE2 && microdnf clean all"
```
