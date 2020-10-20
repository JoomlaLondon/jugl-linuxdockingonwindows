JUGL Workshop on how to setup Docker on Windows to run native linux containers


## Enable WSL 2 feature on Windows.

##### Open PowerShell as Administrator and run:

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

Enable the ‘Virtual Machine Platform’ optional component

##### Open PowerShell as Administrator and run:

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

#### Restart your machine at this point to complete the WSL install and update to WSL 2.


Install the linux kernel package required to update the WSL version to WSL 2.

##### Details of kernel are in below link:

https://docs.microsoft.com/en-us/windows/wsl/wsl2-kernel

##### Installer is at:

https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi


Set WSL 2 as your default version

##### Open PowerShell as Administrator and run:

```
wsl --set-default-version 2
```


#### Install your Linux distribution of choice

Open the Microsoft Store and select your favorite Linux distribution.

Some of the popular one’s are below:

&ensp;&ensp;Debian

&ensp;&ensp;UbuntuSUSE

&ensp;&ensp;Linux Enterprise Server

&ensp;&ensp;KaliLinux


The first time you launch a newly installed Linux distribution, a console window will open and you’ll be asked to wait for a minute or two for files to de-compress and be stored on your PC. All future launches should take less than a second.

You will then need to create a user account and password for your new Linux distribution.
Store this somewhere other than a postit note.

##### Check for the list of linux distributions:

```
wsl -l -v
```

##### Set the distribution to use WSL 2:

```
wsl --set-version <distribution name> <versionNumber>
```

##### Installing Docker Desktop

#### Download Docker Desktop Stable 2.3.0.2 or a later release:

https://hub.docker.com/editions/community/docker-ce-desktop-windows

Accept the default settings during install



Run Docker Desktop.



That’s it.

You have now installed docker on WSL 2.

In a PowerShell window, verify docker is running:

```
docker ps
docker run hello-world
```

Change Image Location of Docker using WSL

Tried several things at first, like using "root-data" param in daemon config, none worked.  
The only thing that did work for me was this:
Open your command prompt:

wsl --list -v

You should be able to see, make sure the STATE for both is Stopped.
```
  NAME                   STATE           VERSION
* docker-desktop         Stopped         2
  docker-desktop-data    Stopped         2
```

Export docker-desktop-data into a file:
```
wsl --export docker-desktop-data "D:\Docker\wsl\data\docker-desktop-data.tar"
```
Unregister docker-desktop-data from wsl, note that after this, your ext4.vhdx file would automatically be removed:
```
wsl --unregister docker-desktop-data
```
Import the docker-desktop-data back to wsl, but now the ext4.vhdx would reside in different drive/directory:
```
wsl --import docker-desktop-data "D:\Docker\wsl\data" "D:\Docker\wsl\data\docker-desktop-data.tar" --version 2
```




CREDITs:
```
[@avinashsingh post on Hackernoon.com](https://hackernoon.com/how-to-run-docker-linux-containers-natively-on-windows-ti1i3uxr)
[StackOverflow](https://stackoverflow.com/questions/62441307/how-can-i-change-the-location-of-docker-images-when-using-wsl2-with-windows-10-h)
```
