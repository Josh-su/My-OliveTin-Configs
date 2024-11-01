# Installation Guide for OliveTin in Unraid
## Search for "OliveTin" in the Community Apps
To get started, open your Unraid web interface and navigate to the Apps section. In the community app search bar, type "OliveTin" to locate the application.

![image](https://github.com/user-attachments/assets/f4f647f3-c62f-49ec-8ce5-c60e9a6e43fc)


## Install the OliveTin App by James Read
From the search results, find the OliveTin app developed by James Read.

![image](https://github.com/user-attachments/assets/7e0bbf81-c33b-4761-b942-57f46a702fb9)

## Activate Advanced View
ensure that Advanced View is activated. This will provide you with more configuration options for the app.

![image](https://github.com/user-attachments/assets/c0a4a8d2-c7fd-4587-b581-32ea98cd1f40)
![image](https://github.com/user-attachments/assets/3abad6d3-3bdf-4890-a06e-1993610ab63a)

## (Optional) Change the Icon URL
You can customize the icon displayed for OliveTin by modifying the Icon URL. The default icon URL does not point to an existing file. If you would like to use the OliveTin logo, you can change it to the following URL:```https://docs.olivetin.app/images/icons/OliveTinLogo.png```

## (Optional) Change the Congig Path
If desired, you can specify a custom path for the OliveTin configuration folder.

## Apply the Settings
After making your desired changes, click on Apply. At this point, the container will attempt to launch. Although it may not find the config.yaml file yet, it will create the directory where your configuration files should be placed

## Create and Upload your config.yaml
Once the configuration folder is established, create your config.yaml file. Upload it to the designated folder. For detailed guidance on creating the config.yaml, refer to the [official documentation](https://docs.olivetin.app/index.html) 

## Test the Setup
At this point, your OliveTin container should be configured with all necessary parameters to function correctly. To verify that everything is working as expected, start the container and check for any errors or issues. Ensure that the application loads properly and that your configuration settings are being applied correctly.

---

# (Optional) Using Docker Command with OliveTin
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



 
