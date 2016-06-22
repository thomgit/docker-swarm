# docker-swarm ( New Swarm Orchestration in Docker Engine v1.12 )
Quick local docker multi-node swarm dev/test setup using virtualbox/vagrant

Based on the slick walksthrought demo by Elton Stoneman at https://www.youtube.com/watch?v=KC4Ad1DS8xU
Refer above video for an insight into the new Docker Swarm clustering, orchestration, load balancing, mesh network etc in Docker Engine 1.12

Pre-requisite: 
---------------
OS: Windows, OSX , Linux ...
System with at least 8GB memory/10GB free hard disk. 
  ** Lower RAM requires edit of Vagrantfile . See Procedure Step 2 below
VirtualBox 5.0.22 and Vagrant 1.8.4 installed ( use latest version available )
All nodes should be in the same subnet ( it will by default be in the below Vagrant setup )

Procedure to setup local VM nodes
---------------------------------
1. Clone the sample Vagrantfile from here to a directory
2. Edit above Vagrantfile using a text editor 
      - adjust v.memory ( 2048 recomended, 1024 or 512 MB as appropiate to your available desktop/laptop RAM ) 
      - adjust 'ip' in config.vm.network "private_network" to your choice or leave the default
      - optional : you can change config.vm.box = "larryli/wily64" to your preffered  ubuntu/linux vagrant box distribution.
                   
3. On the OS command prompt , cd to the directory with above Vagrant file. 
   Load Oracle Virtual Manager application to monitor parallely.

    prompt>  vagrant up   ; This will download 15.10 ( larryli/wily64 ) if this vagrant box is not present and bring up
                            all the 4 VM nodes viz. 1 manager : swarmmgr1 and 3 swarm nodes : swarmnode1, swarmnode2, swarmnode3
4. Log into the swarmmgr1  ( default login: vagrant/vagrant )
    1. install latest docker 1.12 using 
       $ curl -fsSL https://test.docker.com/ | sh
    2. add vagrant user to docker group to avoid using sudo with each docker command
       $  sudo gpasswd -a ${USER} docker
    3. logout and login and check if docker is installed properly 
       $ docker version                ; both client and server should be 1.12.0-rc2 or higher
   Reapeat above step ( 4.1, 4.2, 4.3 )  with all the 3 swarm viz. nodes swarmnode1, swarmnode2, swarmnode3

Our local docker swarm test setup is ready. Now we will configure swarm cluster on this.
Check if iptables or firewalld if enabled- these ports 2377 , 4789 and 7946 should be open on all nodes.

Procedure to configure Docker Swarm cluster
-------------------------------------------
This is amazingly simple ! No KeyValue DB , overlay network setup etc...

1. Log into the swarmmgr1 and run ifconfig to determine the private ip of designated Swarm manager
  It should be 192.168.2.10 if not changed from the default in the sample Vagrantfiel
   $ ifconfig
        ..................
        .................. 
        eth1      Link encap:Ethernet  HWaddr 08:00:27:6d:f7:d2
          inet addr:192.168.2.10  Bcast:192.168.2.255  Mask:255.255.255.0
        ................
2. Configure swarmmgr1 as the Swarm manager 
         $ docker swarm init --listen-addr 192.168.2.10:2377          ; elect this node as Swarm Manager
         $ docker node ls                                             ; list swarmmgr1 as Leader 

3. Login /ssh to each Swarm nodes ( swarmnode1, swarmnode2, swarmnode3 ) and join them to above swarm cluster
         $ docker swarm join 192.168.2.10:2377 


Verify once on swarmmgr ( docker node ls ) all the 4 nodes in the swarm cluster. 
THATS IT !  Follow the superb video walkthrough by Elton Stoneman above for launching a sample service and testing the new v1.12 features:  swarm clustering, fail-over, scaling, mesh routing ...





  

                
                                
    
    
                                           


