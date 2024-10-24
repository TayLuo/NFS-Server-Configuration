# NFS-Server-Configuration
Network File Systemallows a user on a client computer to access files over a network in a manner similar to how local storage is accessed. It's primarily used in Linux and Unix environments to share files and directories between systems on a network. By the end, you will know how to set up a NFS server in your environemnt. 

It has the following features:

	File Sharing: NFS allows for the sharing of files and directories across multiple systems on a network.

	Client-Server Architecture: In NFS, the server exports file systems that are mounted by clients.

	Transparent Access: Files on a remote NFS server appear as part of the client’s local file system, making the access transparent.

	Centralized Data: NFS enables centralized storage and file management, which simplifies backup and file administration.

In this tutorial I will walk you through the configuration process, and provide screenshots along the way. 

Prerequisite: 
	
        Two Linux VMs in your local machine or a cloud environment. For this demonstration, I will spin two VMs on Azure

Need Help on Signing up an account on [Azure](https://azure.microsoft.com/en-us/get-started/azure-portal)

Need Help in creating a Linux VM on Azure, please click [here](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal?tabs=ubuntu)

Here are the following Steps: 

1. Configure a virtual network
   
   In the search box, type "virtual network", Fill up the resource group (same group with your VMs).
<p align="center"> </p>
<img src="https://imgur.com/oj3UhmW.png" height="80%" width="80%" >
<br />

2. Configure a virtual network IP Address
   
   In the IP Address Section, Give an IP address range and its subnet, then click "Review && Create".
<p align="center"> </p>
<img src="https://imgur.com/tdOU4Ep.png" height="80%" width="80%" >
<br />   

3. Create Two Linux VMs on Azure
   
   If you need help with setting up VMs please visit this [Azure Support](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal?tabs=ubuntu) So when you create your VMs, please use the "virtual network" your created. I will provide a screenshot, so you know what to do.
<p align="center"> </p>
<img src="https://imgur.com/ehXVIkq.png" height="80%" width="80%" >
<br />   

4. Configure NFS Server on one VM acting as a server
   
   Once get access to the Server VM, run the following command:
   
   		sudo apt update && sudo apt upgrade -y
   
   		sudo apt install nfs-kernel-server
<p align="center"> </p>
<img src="https://imgur.com/zrGI037.png" height="80%" width="80%" >
<br />   

5. Verify NFS Server installation
   
   Enter the following command to verify the installation of NFS, port 2049 listening state, check if there is anything shared out:
   
   		dkpg -l | grep -i nfs
   
   		ss -ntulp | grep 2049

		showmount -e
<p align="center"> </p>
<img src="https://imgur.com/fMs0z5V.png" height="80%" width="80%" >
<br />    

6. Add a shared directory
   
   Let's create a directory to share out. We also need to prep it for other systems to connect and write by changing permissions.:
   
   		sudo mkdir /share
   
   		sudo chown nobody:nogroup /share
<p align="center"> </p>
<img src="https://imgur.com/F6iKAQE.png" height="80%" width="80%" >
<br />    

7. Configure the /etc/exports directory
   
   Add the line /share *(rw,sync,no_subtree_check) to /etc/exports to share out the directory.:
   
   		sudo vi /etc/exports
   
   		/share *(rw,sync,no_subtree_check)
<p align="center"> </p>
<img src="https://imgur.com/KuFQj4A.png" height="80%" width="80%" >
<br />    

8. Restart the service after configuration
   
   Now it is time to restart the service to see the share.:
   
   		sudo systemctl restart nfs-server.service
   
   		showmount -e
  
9. Configure the client on the second VM
   
   Install the nfs-common client 
   
   		sudo apt update && sudo apt upgrade -y
   
   		sudo apt install nfs-common
<p align="center"> </p>
<img src="https://imgur.com/Wxopjye.png" height="80%" width="80%" >
<br />    

10. Test the mount point to verify we can connect
   
    Run the following command, see if there is connection, it has to be the NFS server's IP:
   
   		sudo mount 192.168.1.4:/share /mnt 
   

<p align="center"> </p>
<img src="https://imgur.com/e7iSSL2.png" height="80%" width="80%" >
<br />    

11. Let's examine the mount point
    There will be a file system mounted to your client machine
   
   
   		df -h /mnt
   
   		
<p align="center"> </p>
<img src="https://imgur.com/22PGam9.png" height="80%" width="80%" >
<br />    

12. Let's verify we can write into this directory
   
    Run the following command:
   
   		sudo touch /mnt/test1
   
   		ls -l /mnt
<p align="center"> </p>
<img src="https://imgur.com/Xidk0u3.png" height="80%" width="80%" >
<br />    

13. Remove the mount point so we can mount it via /etc/fstab
   
    Run the following command add the following line " 192.168.104:/share /mnt nfs defaults 0 0" into the /etc/fstab file
   
   		sudo umount -l /mnt

		sudo vi /etc/fstab

		192.168.1.4:/share /mnt nfs defaults 0 0

	
<p align="center"> </p>
<img src="https://imgur.com/kjs0yWK.png" height="80%" width="80%" >
<br />    
<p align="center"> </p>
<img src="https://imgur.com/qCtY0Hs.png" height="80%" width="80%" >
<br />   

14. Let's check the mount point again
   
 
   
   		df -h /mnt

		

		

	
<p align="center"> </p>
<img src="https://imgur.com/28oxJ8T.png" height="80%" width="80%" >
<br />    
