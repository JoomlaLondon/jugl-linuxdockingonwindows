JUGL Workshop on how to setup Docker on Windows to run native linux containers
CREDIT: @avinashsingh post on Hackernoon.com

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

...Debian...
...UbuntuSUSE
...Linux Enterprise Server
...KaliLinux

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

#### Installing Docker Desktop

Download Docker Desktop Stable 2.3.0.2 or a later release.

Make sure to select below during installation:



Run Docker Desktop.



That’s it.

You have now installed docker on WSL 2.

Verify it by running it in Ubuntu/Linux terminal.
