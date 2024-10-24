# NFS-Server-Configuration
Network File Systemallows a user on a client computer to access files over a network in a manner similar to how local storage is accessed. It's primarily used in Linux and Unix environments to share files and directories between systems on a network. 

It has the following features:

	File Sharing: NFS allows for the sharing of files and directories across multiple systems on a network.

	Client-Server Architecture: In NFS, the server exports file systems that are mounted by clients.

	Transparent Access: Files on a remote NFS server appear as part of the client’s local file system, making the access transparent.

	Centralized Data: NFS enables centralized storage and file management, which simplifies backup and file administration.

In this tutorial I will walk you through the configuration process, and provide screenshots along the way. 

Prerequisite: 
	
        Two Linux VMs in your local machine or a cloud environment. For this demonstration, I will spin two VMs on Azure

Need Help on Signing up an account on [Azure](https://azure.microsoft.com/en-us/get-started/azure-portal)

Need Help in create a VM on Azure, please click [here](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal?tabs=ubuntu)

Step
