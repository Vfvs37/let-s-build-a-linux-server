# let-s-build-a-linux-server

solo project during BeCode Bootcamp

Appendices: documentation in word format, summary, link to a demo


**PROJECT PURPOSE**

The local library in your little town has no funding for Windows licenses so the director is considering Linux. Some users are sceptical and ask for a demo. The local IT company where you work is taking up the project and you are in charge of setting up a server and a workstation. To demonstrate this setup, you will use virtual machines and an internal virtual network (your DHCP must not interfere with the LAN).

You may propose any additional functionality you consider interesting.

**PROJECT REQUIREMENT**

Set up the following Linux infrastructure:

1. One server (no GUI) running the following services:
    - DHCP (one scope serving the local internal network)  isc-dhcp-server
    - DNS (resolve internal resources, a redirector is used for external resources) bind
    - HTTP+ mariadb (internal website running GLPI)
    - **Required**
        1. Weekly backup the configuration files for each service into one single compressed archive
        2. The server is remotely manageable (SSH)
    - **Optional**
        1. Backups are placed on a partition located on  separate disk, this partition must be mounted for the backup, then unmounted

2. One workstation running a desktop environment and the following apps:
    - LibreOffice
    - Gimp
    - Mullvad browser
    - **Required** 
        1. This workstation uses automatic addressing
        2. The /home folder is located on a separate partition, same disk 
    - **Optional**
        1. Propose and implement a solution to remotely help a user

**PERFORMANCES CRITERIA**

The teams must give a sense of confidence to the audience when presenting their demo,
the documentation must be clear, structured and must provide all elements of configuration.

****************************************************************************************************************************

***STEP BY STEP***


- The project encompasses setting up both a server and a workstation in a virtualized environment, meticulously crafted avoid any disruption to the current LAN setup.
- Two virtual machines will be established: one serving as the server, the other as the client. The server will run on Ubuntu Server 22.04 LTS, while the client will utilize Ubuntu Desktop 22.04 LTS.
- The server will function without a graphical interface, prioritizing resource efficiency. Meanwhile, the workstation, adorned with a GUI, will accommodate vital applications essential for day-to-day library tasks.


![image](https://github.com/Vfvs37/let-s-build-a-linux-server/assets/155911615/984ea7bd-78f6-445d-8165-e052f2f76003)


** LINUX SERVER (no graphical interface)**

Ubuntu 22.04.4 TLS

![image](https://github.com/Vfvs37/let-s-build-a-linux-server/assets/155911615/a7becb3f-e1f5-44b6-8a36-048e3c3ab887)

As you can see, there's no graphical interface. All the configurations are made in a black interface, which let you directly acces to the terminal once the configurations are done.

But first of all let's quickly see how to install the whole thing to create our VM.
1. after installing VirtualBox by following this tutorial: https://www.youtube.com/watch?v=nwjZWHou8u0
2. download the ISO file on https://ubuntu.com/download/server
   ![image](https://github.com/Vfvs37/let-s-build-a-linux-server/assets/155911615/a8ac98c3-2903-4191-88f6-bc724b759e31)
  it should looks like that on your files
3. then follow this tutorial to creat your VM for a good installation: https://www.youtube.com/watch?v=IJ45EzEDimE&list=PL2ZsxaiKPFW-MGWyoPaqMGDXBlj43jv6B&index=9
4. then add personnal specifications, for me it was the following ones, then confirm everything

* a bit more than 2 GB of RAM
* 25 GB of storage
* 2 CPU cores
* Bridged network adapter
* Ubuntu 22.04 LTS Desktop Edition
* User: vfvs
* Password: kamkar24
* IP Address: 10.0.2.15
* Hostname: Ubuntu serber

5. then put some power on your machine and start configurate it, chose your language and let all the default settings for now, no need to change nothing

***1: configurations***

1. updating by using "sudo apt update" command
2. upgrating by using "sudo apt upgrade" command
3. install SSH by using "sudo apt install openssh-server" command
4. enable SSH by using "sudo systemctl enable ssh" command, then "sudo systemctl start ssh"
5. configure UFW firewall by using "sudo uf allow ssh" command, then "sudo ufw enable"
6. if needed, install addintional softwares by usin "sudo apt install -name_of_the_software- ". For this project for exemple i added GIMP and LibreOffice.
7. set up Remote 

[* if for some reson you need infos about your server, use the "IP A" command

![image](https://github.com/Vfvs37/let-s-build-a-linux-server/assets/155911615/863138dd-c35d-49e4-a731-ff3556874a74)


As you can see, there is 2 parts. The seconde one is about Enp0s3 interface. Let's explain it:

The use of the "enp0s3" interface typically refers to a specific network interface on a Linux system. This naming convention is often seen in modern Linux distributions where network interfaces are named based on their physical location or other identifying factors. 

For example, "enp0s3" might indicate the third network interface (3) on the first (0) PCI bus slot (p0) in the system. 

As to why this interface is being used, it could be due to several reasons:

* **Automatic Assignment**: The system might have automatically assigned this interface to handle network communication based on its configuration or detection during installation.

* **Static Configuration**: It could be statically configured to handle specific networking tasks within the system or network environment.

* **Virtual Machine Configuration**: In a virtualized environment, such as VirtualBox or VMware, "enp0s3" might be the default interface name given to the virtual network adapter connected to the host's physical network.

* **Hardware Configuration**: It might correspond to a specific physical network adapter installed on the system.

The precise reason would depend on the context of the system's setup and configuration. 
In my prject i used it and the ip adress is 10.0.2.15/24 because in VirtualBox we'll modifu-y some settings on the VM by add another interfaceto enable the configuration of a static IP address, facilitating SSH and GLPI setup.]



8. activating the network interface on the network settings and chosing the private host network, wich name is "virtualBox Host-Only Ethernet Adapter"
9. then manage manually your static ip adress
10. to finish, type the "ip a" command on your terminal to see if everythig's managed correctly

***2: DHCP server***

    


