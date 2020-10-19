# JUGL Workshop on how to setup Docker on Windows to run native linux containers
# CREDIT: @avinashsingh post on Hackernoon.com
#
# Enable WSL 2 feature on Windows.


Open PowerShell as Administrator and run:


```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```


Enable the ‘Virtual Machine Platform’ optional component

Open PowerShell as Administrator and run:

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

Restart your machine at this point to complete the WSL install and update to WSL 2.




