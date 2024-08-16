# Containers

## Need for Containers


### Scenerio 1 : Traditional application deployment
![image](https://github.com/user-attachments/assets/b22141ce-95e6-4f9f-a152-62a9e6dd8c0d)

Problem with this approach : 
- ***Wastage of resources***

### Scenerio 2 : Running multiple application on same hardware

To solve the resource underutilization problem, we can run multiple applications on the same hardware

![image](https://github.com/user-attachments/assets/9705e4db-af66-4586-9177-2cc3afb6ff15)

Problems:

- Lets say if App-B misbehaves and starts consuming more resources (9GB RAM/9CPU) which will inturn put burden on App-A. ***This is happening as there is no fair scheduling of resources***

- Somebody who has access to App-A can go to operating system level and can access App-B & vice-versa. ***This is happening as applications are not well isolated***

### Scenerio 3: Virtualization

To solve the above issues we can create multiple VM's and run each application on one VM

![image](https://github.com/user-attachments/assets/464b38bc-4506-4a47-a560-0d59eb7e16a1)

- For each VM a portion of hardware is allocated, thus achieving fair scheduling of resources.
- Each App is hosted on seperate VM having their own OS, thus achieving isolation
Additional Info: Each EC2 instance is a VM and securitywise they are so isolated that lets say Zomato is working on EC2-1 & Swiggy working on EC2-2 of the same H/W still they cannot access each others data infact they dont even know that they are working on the same H/W.

Problems :
- Since we are running so many OS, the licensing and maintainence cost will increase leading to ***increase overall operational expenditure***
- Running so many OS will ***lead to wastage of resources***
- If we want to migrate App-A from VM-1 to some other VM, then replicating the same environment is challenging sometime, ***i.e lift & shift is challenging***

### Scenerio 4: Containers

Containers say whatever is needed inorder to run an Application just bundle them and not the entire OS (As OS may have other things installed like printer drivers which may not be needed to run the current Application)
![image](https://github.com/user-attachments/assets/26ccbe2a-a447-4aa7-82ac-998d5948e4aa)

- This will not only achieve lift and shift but also reduce OpEx
- Additionally containers avoid wastage of resources as containers are very much light in weight compared to OS

![image](https://github.com/user-attachments/assets/c4992af6-66e9-4f3c-b0ee-f853656eb0ec)

Container-Runtime is a software that is needed to support containerization. There are multiple in market
- Docker
- Rock-It
- Containerd
- cri-o

Out of these Docker is the most famous one. To be honest it has become a synonym for Containers

#### Lets do them in action
    1. Open Terminal of ubuntu Machine
    2. Check if docker is present : docker --version / docker -v
    3. If not installed then : sudo apt install docker.io
    4. sudo su (All docker commands need root privileges)
    5. docker run -d nginx (run a webserver container from the image)
    6. docker ps (show the running containers)
    7. ps -aux | grep -i nginx (List down all nginx process)
    8. kill -9 pid_of_nginx (root process)
    9. docker ps (You will see no active containers running)

From this we can conclude that ***a container is nothing but an OS process*** (since we are able to kill the container at a OS process level)

But how can a process achieve
- fair sceduling of resources like VM do
- keep Apps isolated like VM do

#### Namespace & Cgroup:
*Namespace* is a linux OS feature which partitions the kernel resources in such a way that one set of process sees only one set of resources while another set of process see different set of resources. ***So Namespace decides which of resource a process can see***

*Cgroup (Control groups)*: Is again a linux OS feature which ***decides how much resource a process can see***

And that is what exactly hypervisors do, so containers are no new invention. Its just utilizing linux kernel feature and thats how they behave like VM





