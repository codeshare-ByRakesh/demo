



profiles             ---------------                           1.       https://github.com/sweetcbk           2.     https://github.com/Keshari07   3.  https://github.com/NotHarshhaa

4.  https://blog.prodevopsguy.xyz/?source=top_nav_blog_home  -----short tutuorial


Date : 1 May,2023
---------------------------------------------
hadoop : EMR
vm : EC2 instance
database : RDS 
nfs server shares the data on the network, in this given example we are storing data from node 1 and node 2 on storage machine

how live migration works in aws
3 machines
1. node 1
2. node 2
3. storage server

# node 1
/----Ip address given to this node------/
-> 192.168.10.1

/----commands which are need to run on node1-----/
-> sudo su
-> apt update
-> apt upgrade
-> apt install libvirt
-> apt install virt-manager
-> apt install qemu-kvm(window server 2016 is installed by default)
# open virt-manager and connect to centralised server
    then start the virtual machine -> window server 2k16
-> enable ssh on node1
    -> sudo vim /etc/hosts
    -> sudo vi /etc/ssh/sshd_config
        PermitRootLogin yes
        PermitRootLogin without-password
    -> sudo systemctl restart ssh 
# generate keygenerator for the local user
-> ssh-keygen -t rsa
# generate keygenerator for the root user
-> sudo ssh-keygen -t rsa
# enable ssh root login for ubuntu
-> sudo passwd -l root
# generate password for root
-> sudo passwd
    -> enter UNIX password
    -> re-enter UNIX password
# we need to copy public key of node1 to node2
-> sudo ssh-copy-id -i /root/.ssh/id_rsa.pub root@node2
    -> yes to confirm
    -> enter node2 password: 
    -> node1 public key is copied successfully to node2

# add connection in qemu-kvm 
-> click on add connection
    -> give connection type


# node 2
/----Ip address given to this node------/
-> 192.168.10.2

/----commands which are need to run on node2-----/
-> connect hypervisor node 2 to nfs share
-> enable password less ssh, from node 1 to node 2 and reverse, enable root login through ssh
-> then migrate
-> enable ssh on node1
    -> sudo vim /etc/hosts
    -> sudo vi /etc/ssh/sshd_config
        PermitRootLogin yes
        PermitRootLogin without-password
    -> sudo systemctl restart ssh
# generate keygenerator for the local user
-> ssh-keygen -t rsa
# generate keygenerator for the root user
-> sudo ssh-keygen -t rsa
# enable ssh root login for ubuntu
-> sudo passwd -l root
# generate password for root
-> sudo passwd
    -> enter UNIX password
    -> re-enter UNIX password
# we need to copy public key of node2 to node1
-> sudo ssh-copy-id -i /root/.ssh/id_rsa.pub root@node1
    -> yes to confirm
    -> enter node1 password: 
    -> node2 public key is copied successfully to node1

/-----attach node2 vm to centralised storage server------/
    -> storage -> add pool -> name : "default" -> type : netfs : Netword Exported directory
    -> add ip address : 192.168.10.5 and directory name : 
    # to check if storage server is attached to nhi
    -> showmount -e 192.168.10.5

# storage
/----Ip address given to this node------/
-> 192.168.10.5

/----commands which are need to run on storage-server-----/
-> sudo su
-> apt install nfs-server
    # after installing the server we need to create this file and add the directory which need to be shared
        -> vim /etc/exports
            /vm-store *(rw no_root_squash) 
            * : means that it will accept request form all ip address
            rw : means it is read and write operations are performed
            no_root_squash : by default it does not treat root as root user
        -> systemctl restart nfs
        -> systemctl restart nfs-server
        -> showmount  -e 192.168.10.5
    # check content of the shared directory we use
        -> ls /vm-store/
    
how to change static ip address for given machine and allocate to dhcp ip
# edit the given file
    -> sudo vim etc/netplan/01-netcfg.yaml
        change dhcp4 : no to yes
        comment the ip address the hardcoded address




# Containers : lighweight execution units 
physical server -> VirtualMachines -> Containers

issue with VMs: it requires an Operating system, when a VM starts,first the os start and then the application starts,
so it takes some time to respond to client therefore, there is a delay in VMs
VMs are not detachable
container images are only Read-only

this can be overcome by using containers: 
containers do not require the complete os, when a container starts, no os Booting required. this container execution is fast as compared to VMs.

to run containers:
1. we need container runtime
2. containers runtime is a software which allows you to create,start, stop,i.e. manage containers

ex. docker,rkt,podman,containered
-> container provides isolation

how container is created:
-> need an image
-> contains os based libraries
-> functions + platofrm(middleware) + application

how to create a container
# to create a container we use, docker environment
to download docker, we go to the docker repository 
-> hub.docker.com

how to install docker
-> sudo apt install docker.io -y

how to check docker installed or not
-> docker

how to start docker service
-> sudo systemctl start docker

how to enable docker on autostart, so that next time when system start,docker service get's started automatically
-> sudo systemctl enable docker

how to check running container on the given machine
-> sudo docker ps
how to check all the containers in running or stopped state
-> sudo docker ps -a

container LifeCycle
-> create -> start -> execute(running mode) -> stop/pause

how to check local docker images on the system
-> sudo docker images

how to pull remote image on the local system
-> docker pull {image_name}
ex. docker pull centos

how to run a docker image
-> docker run {image_name}
ex. docker run centos

how to delete a container
-> docker rm {container_name/container_id}
ex. docker rm centos

how to run a container terminal in interactive mode
-> docker run -it {container_name/container_id}

how to give custom name to a container
-> docker run -ti --name {new_name} {container_name/container_id}
ex. docker run -ti --name c1 centos

how to stop a container which is in running state
-> docker stop {container_name/container_id}

difference between docker run or start
docker start : only used to start a stopped container
docker run : used to create new instance of the base image

how to bring docker running in backgroup to foreground
-> docker attach {container_id/container_name}

note: when no tag is mentioned while pulling an image, it automatically picks up the image latest image
-> docker pull {image_name}:tag

when docker is installed on the system, then it creates a virtual adapter by the name of docker0, and we can check that by using command
-> ip a

how to get detailed view of a docker image
-> docker inspect {container_name/container_id}

when container is stopped, then it's all resources get's released,except storage
default ip for docker is 172.17.0.1

how to run apache2 server using container
-> docker run --name web1 httpd

how to run apache2 server using custom port
-> docker run --name web1 -p 8000:80 httpd
    machine-ip:8000
    container-ip:80

2 may,2023
-----------------------------------------------

location where websites are hosted in httpd container
-> /usr/local/apache2/htdocs/

how to host a website inside httpd container
step 1: first create a container of httpd
step 2: create html file 
step 3: run the container with specific parameters
step 4: paste the custom file in in the container

ex. 
# create the webpage
->  sudo vim index.html
# run the docker container
->  sudo docker run --name web1 -p 9100:80 -d httpd:tag(default: latest)
# see if container is running or not
->  sudo docker ps 
# check if webpage is running or not
->  browser -> visit the ip address : localhost:9100
# copy the file in the container page
->  sudo docker cp index.html web1:/usr/local/apache2/htdocs/


# issue with this approach is that everytime website content is change we need to copy to the container, which is not a practical approach
# to overcome this we mount our developer directory to the container, this process is known as volume mapping

how to do volume mapping
# create the directory for the website
-> sudo mkdir webdata && cd webdata
-> sudo vi index.html
-> cd .. 
-> sudo docker run --name web2 -p 8100:80 -v /webdata/:/usr/local/apache2/htdocs/ -d httpd
    # description of the command
    --name : gives name to the docker container
    -p : assign the ports on local machine: inside container
    -v : maps the volume present on local machine to the location present inside the container

what if same dir is mapped to another container instance with different port,this will run new container with different port,
this approach is used to do load balancing
-> sudo docker run --name web3 -p 8101:80 -v /webdata/:/usr/local/apache2/htdocs/ -d httpd

# running container on web_instance in AWS RedHat
instance commands
->  yum install podman
->  sudo podman images
->  sudo systemctl status podman  
->  sudo systemctl start podman
->  clear
->  sudo podman ps
->  sudo mkdir /weddata  
->  cd /webdata
->  sudo vi index.html
    welcome to the podman based website
->  sudo podman run --name w1 -p 80:80 -v /webdata/:/usr/local/apache2/htdocs/ -d httpd
->  select the registry image where to pull the httpd image
    [ ]registry.access.redhat.com/httpd:latest
    [ ]registry.redhat.io/httpd:latest
    [*]registry.io/httpd:latest
->  sudo podman ps
# if container gets stopped without any reason, to find the reason we need to check the container logs
# to check container logs
->  sudo podman logs w1 -> (no logs found)
->  sudo podman images -> (image was download successfully)
# to check redhat logs
->  sudo tail -20 /var/log/messages/
-> sudo getenforce -> Security Enhanced Linux (it doesn't allow to run containers) -> enforcing mode
-> sudo setenforce 0 -> Permissive mode
-> sudo podman rm w1
# now try running the linux after putting SElinux in Permissive mode
-> sudo podman run --name w1 -p 80:80 -v /webdata/:/usr/local/apache2/htdocs/ -d httpd -> container running successfully after disabling Security Enhanced Linux

# running docker container for mysql on ubuntu based instance 
-> apt install docker.io
-> sudo systemctl start docker
-> sudo docker run --name db1 -ti -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -d mysql 
    # description of the command
    -e : set the password for default root account 


# how to create docker image from ubuntu image file
# there is direct root access inside, the container
# docker file name should be Dockerfile
# dont delete the file as it will create each and every layer 
# two images can't have the same name
# use "double" quotes whenever we pass arguments inside CMD command in Dockerfile, also the space to separate the arguments


to delete a docker image we use
-> sudo docker rmi [image_name/image_id]

to delete a container we use
-> sudo docker rm [container_id/container_name]

-> code
    FROM ubuntu
    RUN apt update -y
    RUN apt install python2.7 -y
    RUN mkdir /App1
    COPY hello.py /App1/
    CMD [ "python2.7" , "/App1/hello.py" ]

# tell docker to build image
# -t : it gives name:tag
-> docker build -t [image_name] [path]
ex docker build -t myapp:v1 ./

docker creates the image in layer format

------------------------
|   run file           |   :   layer 6
------------------------
|   copy file          |   :   layer 5
------------------------
|   mkdir /App1        |   :   layer 4
------------------------
|   install python 2.7 |   :   layer 3
------------------------
|   update os          |   :   layer 2
------------------------
|   os image           |   :   layer 1
------------------------

# creating another image in docker for python

-> code
    FROM ubuntu
    RUN apt update -y
    RUN apt install python2.7 -y
    RUN mkdir /app2
    COPY prog1.py /app2/
    CMD [ "python2.7" , "/App1/prog1.py" ]

-> docker build -t myapp2 .

# creating another image in docker to run bash script

-> code
    FROM ubuntu
    RUN apt update -y
    RUN mkdir /script
    COPY script.sh ./script
    CMD [ "./script/script.sh" ]

-> docker build -t script1 .

how to save container as image file
# it doesn't execute anything, it just saves the state of the container
# sudo docker run --name ub1 -ti ubuntu
    -> mkdir /test
    -> cd /test
    -> vi s1.sh
    -> cat > s1.sh
    -> ls
    -> chmod +x s1.sh
    -> ./s1.sh

# to save a container as a image, it has to be in stopped state
-> sudo docker commit [container_name] [image_name]

# now we can recover that file in by running the backup file, just in case if that container gets deleted


# how to save a container into a tar file format and send it to someone else

# method 1: using tar file format
-> container -> .tar file

# this command will make the tar file in the current directory where this command has been executed
-> sudo docker save myapp1 > myapp1.tar

# how to recover container from tar file to docker container
-> sudo docker load < myapp1.tar  

# method 2: upload this container to docker hub, and pull it from remote repository
-> create account on docker hub 
-> create repository
-> give name to repository -> give description -> choose public, or private -> 
-> sudo docker login 
    -> username: [username]
    -> password: [password]
-> sudo docker tag myapp1 [username/repository_name]
-> sudo docker push [username/repository_name]


----------------------------------
3 may 2023
----------------------------------
# Container networking
# what is the default ip and networking mode for a container
# default IP : 172.17.0.1
# default mode : Bridged (acts as a NAT mode, only single side connection is allowed, but this mode allow port mapping)
# default adapter : docker 0 : bridge mode

how to create new docker network?
-> sudo docker network create [network_name]
-> ex. sudo docker network create app1

how to find help regarding docker network commands
-> sudo docker network -h

# in order to run our application we pack it inside a custom image,which will be used to run container based on this image

# python script to connect with database
    import mysql.connector
    mydb = mysql.connector.connect(
       host="hpcsa-db"
       user="root"
       passwd="password"
       auth_plugin="mysql_native_password"
      )
    testcursor = mydb.cursor()
    test.cursor.execute("Create database studentdb")
    test.cursor.execute("SHOW DATABASES")
    for x in testcursor:
    print(x)
-> save it as test.py

# creating the Dockerfile
    FROM python 3.9
    RUN pip install mysql-connector-python
    CMD [ "python" , "test.py" ]

-> sudo docker build -t py-sql .

# we do not require port mapping, as networking is being done inside containers only
-> sudo docker run --name hpcsa-db -e MYSQL_ROOT_PASSWORD=password -d mysql

-> sudo docker run --rm py-sql

# we need to stop previous running container
-> sudo docker stop hpcsa-db

# restarting the containers with custom network
-> sudo docker run --name hpcsa-db --network hpcsa -e MYSQL_ROOT_PASSWORD=password -d mysql

# now running the py-sql container
-> sudo docker run --network hpcsa --rm py-sql

# conclusion
-> create separate directory
-> copy test.py in that directory 
-> edit test.py and specify hostname,username,password
-> create Dockerfile
-> build image from docker file 
-> create docker network for mysql and py-sql
-> start mysql container with custom network 
-> start application container within the same network 

# issues with single container 
# not scalable
# single point failurs

# to resolve this we use container clusters
# we have 2 options to form clusters, both are opensource
1. docker swarm : only support the docker runtime
2. kubernets(k8s) : can be used with other container runtime


# kubernets

# Master node
        |_______> Container_node1
        |_______> Container_node2
        |_______> Container_node3

container_nodes are known as worker nodes.
it keeps the replicas of the worker nodes.


# kubernetes clusters
-> it is a collection of container working as woker nodes, which are also replicates from time to time by master node, but here, master node becomes a single
point failure, in order to prevent this we use multi master system

-> in order to work on cloud, kubernetes has to be used with clouse controller

# Kubernetes Components
1. kube-apiserver
2. etcd : it is database,it store all the datam,it's configuration data, it's state and it's metadata
3. kube-scheduler
    pod: when group are made on the basis of application, and these are assigned with the ip, this group is known as pod
4. kube-controller-manager: components that runs on master
    1. node controller
    2. replication controller
    3. Endpoints controller
    4. Service Account & Token controller
    5. Cloud Controller Manager
        1. node controller
        2. router controller
        3. service controller
        4. Volume Controller
5. kubelet : acts as a worker node or client node, it does not manage containers which are not created by kubernetes 
6. kube-proxy : by default, ip's of the container are not fixed, to prevent this problem we use kube-proxy, it updates and manages the ip address of the
container present inside pod

                                                _____________________________________
                                                |  default ip: 172.17.0.2           |
                                                |  default adapter: docker0         |
                                                |  default network : 172.17.0.1     |
                                                |  worker node 1                    |
                                                |___________________________________|
                                                _____________________________________
                                                |  default ip: 172.17.0.2           |
                                                |  default adapter: docker0         |
                                                |  default network : 172.17.0.1     |
                                                |  worker node 2                    |
                                                |___________________________________|
                                                _____________________________________
                                                |  default ip: 172.17.0.2           |
                                                |  default adapter: docker0         |
                                                |  default network : 172.17.0.1     |
                                                |  worker node 3                    |
                                                |___________________________________|

# we need to install flannel network plugin, which will be used to define the cidr network for the pod networks
# pod ip address are machine specific
# pod1 default ip address: 10.244.1.1
# pod2 default ip address: 10.244.2.1
# pod3 default ip address: 10.244.3.1

how to get nodes in the cluster
-> kubectl get nodes

how to get cluster-info
-> kubectl cluster-info

how to start a cluster
-> sudo kubeadm init --apiserver-advertise-address 192.168.18.1 --pod-network-cidr=10.244.0.0/16
# this command will return a token in the end,we need to store the token, which will be executes on the client machines

creating a directory
-> mkdir .kube

copy admin config to .kube directory
-> sudo cp /etc/kubernetes/admin.conf ./kube/config

get ownership of the .kube folder
-> sudo chown {user}:{group} ./kube/config 
-> sudo chown uadmin:uadmin ./kube/config 

get running nodes
-> kubectl get nodes (by default it shows the container running on local machine)

get running pods
-> kubectl get pods
-> kubectl get pods --all-namespaces(this will return all the namespaces running on local machine,which are running as virtual environment)

to run token on the client nodes
# first copy the token to the client nodes
-> scp kube-join worker1:/home/uadmin
-> scp kube-join worker2:/home/uadmin


# on worker1
-> sudo ./kube-join
# on worker2
-> sudo ./kube-join

to see the running nodes
-> kubectl get nodes
# master node role will be, control-plane
# worker1 node role will be, ready
# worker2 node role will be, ready


----------------------------------
4 may 2023
----------------------------------

Docker vs kubernetes
-> docker is designed to be run on single machine, therefore we can't scaleup the application using docker commands
-> also known as container orchestration

why we need cluster of containers
1. for load balancing
2. to avoid single point failures

replica sets: will create clone of pods

deployment is just a management unit which wont be getting any ip address

to access a app,we can't give pod ip, reason being there are many pods running at the same time, and we are not aware
which pod is running what application, we also can't use deployment reason being, it doesn't have any ip address.

so to overcome this issue, we use service.
-> it is an entity that allows users to access an application running(deployment) in a kubernetes cluster
-> deployment does not get an ip address, thus service is created which get's an ip address

types of services
1. cluster IP
    -> this service gets cluster ip address
    -> this can be access by other services within that cluster only

2. node port 
    -> this type of service is used for application which will be accessed by users
    -> this application port is mapped on all node ip address within the cluster

3. load balancer
    -> this type of service is used mostly in cloud
    -> the load balancer assigns an ip address to the service 
    -> mostly services that will be accessed from internet use this type

# commands to run after installing kubeadm

# this command will advertise the ip address of the master node
-> sudo kubeadm init --apiserver-advertise-address 192.168.10.1 --pod-network-cidr=10.244.0.0/16
    # parameters
    --apiserver-advertise-address : master ip address which is needed to be advertised
    --pod-network-cidr : this parameter will give ip address to the pods, when they will be runned

-> for cluster create vm in respective modes
    -> first network adapter: NAT mode
    -> second network adapter: Host mode

# whenever cluster is created we need to create new directory in user's home directory, and we need to create new directory
and create a new token everytime, whenever kubernetes server is need to be restarted

# create the directory
-> mkdir .kube

# copy the token file to required directory
-> sudo cp /etc/kubernetes/admin.conf ./kube/config

# change the ownership of the token file
-> sudo chown uadmin:uadmin ./kube/config

# run this command to get th info on running nodes
-> kubectl get nodes

# get info about pods of the master
-> kubectl get pods --all-namespaces
-> kubectl apply -f https://url
-> kubectl get pods --all-namespaces

# to reset the cluster
-> sudo kubeadm reset

# remove the directory
-> rm -rf .kube
-> sudo kubeadm init --apiserver-advertise-address 192.168.10.1 --pod-network-cidr 10.244.0.0/16
# copy the new toke to the file
-> sudo vim kube-cluster-join
-> mkdir .kube
-> sudo cp /etc/kubernetes/admin.conf .kube/config
-> sudo chown uadmin:uadmin .kube/config
-> kubectl get pods --all-namespaces
-> cat kube-cluster-join

# to run this file over ssh on the worker node 
-> scp kube-cluster-join 192.168.10.5:/home/uadmin
    enter password for the worker1 node:
    worker1 : run this file as sudo user in worker 1 node
-> scp kube-cluster-join 192.168.10.6:/home/uadmin
    enter password for the worker2 node:
    worker2 : run this file as sudo user in worker 2 node

# to check all the running nodes on the system we use
-> kubectl get nodes

# to get all the running pod namespaces(it is a logical grouping)
-> kubectl get pods --all-namespaces
-> kubectl get deployment

# to get running services
-> kubectl get svc/service
-> kubectl create deployment
-> kubectl create deployment web1 --image=httpd --port=80

# to check deployment
-> kubectl get deployment

# to get pods running (-o wide will show additional columns)
-> kubectl get pods -o wide

# how to get information about a deployment
-> kubectl describe deployment_name

# how to describe pod or deployment
-> 
# how to scale up a deployment
-> kubectl scale deployment web1 --replicas=4

# to check how many replicas are created(gives information about all pods)
-> kubectl get pods -o wide

# to get information about 1 specific pod 
-> kubectl describe pod

# sometimes it may take longer time, because, container starts downloading the image, which is a time taking process
# how to scale down the deployment, scaling down will delete the containers
-> kubectl scale deployment web1 --replicas=1

# how to create a cluster ip service
-> kubectl expose deployment web1

# ip given by this service, can only be accessed by the machine present inside that machine

# how to delete a service
-> kubectl delete svc web1

# how to create node port service
-> kubectl expose deployment web1 --type=NodePort

# how to create LoadBalancer service
# in order to work this, we need a loadbalancer to work
-> kubectl expose deployment web1 --type=LoadBalancer

cluster IP -> node IP -> LoadBalancer IP 
-> kubectl delete pod web1-id

# how to get replica set
-> kubectl get rs

# kubernetes cluster, rollup feature
ipaddress -> service -> deployment -> replicaset -> pod1
                                                 |_ pod2
                                                 |_ pod3

in rollup feature, a replica of deployment is created then, all the connection are shifted to new replica set of the deployment
replica and main deployment branch is deleted. then deployment service is updated with new image. and container are created form that
new image

this also allows to roll back the changes, just in case if there is any error with new image file, and container are not bein created

----------------------------------
5 may 2023
----------------------------------

Pods : group of containers running on a machine is known as pod
deployment : group of pods is known as deployment

check for docker images
-> sudo docker images

create a directory
-> mkdir web-app1
-> cd web-app1

check for nginx image
-> go to hub.docker.com
-> search for nginx image
-> look for documentation how to use it

location where nginx host the website
-> /usr/share/nginx/html/

create the web-app 
-> sudo vi index.html -> write web contents to the app

create the Dockerfile
-> sudo vi Dockerfile

content inside Dockerfile
-> FROM nginx:latest
-> COPY index.html /usr/share/nginx/html/

create the image
-> docker build -t web-app1:v1 ./

running container form the image
-> docker run --name web1 -p 8100:80 web-app1:v1

# create repository on docker hub by the name of the web-app

now rename the docker image, new name should be same as repository name
-> sudo docker tag web-app1:v1 [new_name]

login into docker hub account
    enter username:
    enter password:
    
push the image to the docker hub
-> sudo docker push [new_img_name]

# kubernetes part 

create kubernetes deployment
-> kubectl create deployment web-app1 --image=[image_name_from_cloud] --port=80 --replicas=2

-> kubectl get pods
-> kubectl get pods -o wide

create a service
-> kubectl expose deployment web-app1 --type=NodePort

check the services running
-> kubectl get services

now if we update our app, we need to repeat the steps
1. create docker img of the updated file
2. push the updated img file to docker hub
3. update the image in kubernetes

to automate this process we use jenkins

update the image in kubernetes we use
-> kubectl set image deployment/web-app1 web-app1=[image_name_from_cloud]

how to roll back a deployment
-> kubectl rollout undo deployment/app --to-revision=2

create new namespace (ns=namespace)
-> kubectl create ns hpcsa

assigning this name space to deployment
-> kubectl create deployment hpcsa-app --image=httpd --port=80 --namespace=hpcsa --replicas=2

how to check if deployment is create with our namespace
-> kubectl get deployment --namespace=hpcsa
or
-> kubectl get pods --all-namespace

# note : without --namespace argument, we get output of default namespace

# now to create a service for the given deployment we have to give namespace parameter
-> kubectl expose deployment hpcsa-app --type=NodePort --namespace=hpcsa

# note: deleting the deployment wont delete the service automatically, we have to create it manually

-> kubectl delete svc web-app1
-> kubectl delete deployment hpcsa-app

# how to create yaml file
-> create mkdir /App1
-> cd App1

# create yaml file
-> sudo vi web-app1.yaml

    apiVersion: apps/v1
    kind: Deployment
    metadata:
    name: web-app1-dep
    spec:
    selector:
        matchLabels:
        app: web-app1
    replicas: 2 # tells deployment to run 2 pods matching the template
    template:
        metadata:
        labels:
            app: web-app1
        spec:
        containers:
        - name: web-app1
            image: [image_name]
            ports:
            - containerPort: 80

Create a deployment using yaml file
-> kubectl apply -f web-app1.yaml

check deployments
-> kubectl get deployment

check replicas
-> kubectl get rs
-> kubectl get pods
-> kubectl describe deployment web-app1

# create service with cluster IP yaml file
-> sudo vi web-app1-svc.yaml

    apiVersion: v1
    kind: Service
    metadata:
      name: web-app1-dep
    spec:
      selector:
        app: web-App1

      ports:
        - name: http
          protocol: TCP
          port: 80
          targetPort: 80

# create service with NodePort IP yaml file
-> sudo vi web-app1-svc.yaml

    apiVersion: v1
    kind: Service
    metadata:
      name: web-app1-dep
    spec:
      type: NodePort
      selector:
        app: web-App1

      ports:
        - nodePort: 31000 
          port: 80
          targetPort: 80

# create service with NodePort IP yaml file
-> sudo vi web-app1-svc.yaml

    apiVersion: v1
    kind: Service
    metadata:
      name: web-app1-dep
    spec:
      type: LoadBalancer
      selector:
        app: web-App1

      ports:
        - nodePort: 31000 
          port: 80
          targetPort: 80

# how to create a deployment in dry run
-> kubectl create deployment web-app2 --image=[image_name_from_cloud] --port=80 --replicas=2 --dry-run=client -o yaml > [file_name].yaml

# kubectl cordon : is used to stop pod, and no more work will be scheduled upon them
-> kubectl cordon worker2

# making more deployment to see of work gets divided to worker2 or not
-> kubectl create deployment h1 --image=[image_name_from_cloud] --port=80 -replicas=4 

# after this pod with no workload can be removed safely
-> kubectl delete pod #pod-id

# how to remove temporary scheduleding or uncordon it, so that workload can be distributed to it
-> kubectl get pods -o wide

