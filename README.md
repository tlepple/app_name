# app_name
testing some modular items

---
---

# Build a docker container to execute this build


###  Setup some directories on mac for assets to be shared between docker container and MacOS


```
# create a directory in OS for bind mount:
cd ~
mkdir -p ./Documents/cloud_stuff/docker_stuff

# create a softlink to this directory
sudo ln -s /Users/$USER/Documents/cloud_stuff/docker_stuff ~/fishermans_wharf

```

## Create the docker container and mount the directories


```
# Create a docker volume to persist data
docker volume create app_name_vol1

# Create the docker container
docker run -it \
  --name aws_build_cnt \
  --mount source=tapp_name_vol1,target=/app \
  --mount type=bind,source=/Users/$USER/Documents/cloud_stuff/docker_stuff,target=/root/fishermans_wharf \
  centos:7 bash 
    
```


### Pull the repo

```
#install git
yum install -y git
cd /app    



git clone https://github.com/tlepple/app_name.git

cd /app/app_name

git submodule init
git submodule update --force --recursive --init --remote

```


### Edit the demo.properties file for your updates and set AMI and Region you are running in

```
vi /app/app_name/bin/provider/aws/demo.properties
```

### Sample demo.properties

```
###############################################################################
#  Updates go here:
###############################################################################
OWNER_TAG=<your owner tag>
BIND_MNT_SOURCE="/Users/<your MacOS UserName>/Documents/cloud_stuff/docker_stuff"
HOST_PREFIX="demo-cdpdc7-1-3-"

# Below are the instance types for the various services. It's recommended to use
# the defaults, however you can change them below, if needed.
ONE_NODE_INSTANCE=m4.4xlarge

#PROJECT_TAG="'""personal development""'"
ENDATE_TAG=permanent

# set the docker bind mount properties
BIND_MNT_TARGET="/fishermans_wharf/"

# N. Virginia - us-east-1
#AMI_ID=ami-02eac2c0129f6376b
#AWS_REGION=us-east-1

# Ireland eu-west-1a
#AMI_ID=ami-0ff760d16d9497662

# Ohio us-east-2
#AMI_ID=ami-0f2b4fc905b0bd1f1
#AWS_REGION=us-east-2

#N. California us-west-1
#AMI_ID=ami-074e2d6769f445be5
#AWS_REGION=us-west-1

#Ireland
#AMI_ID=ami-0ff760d16d9497662
#AWS_REGION=eu-west-1

# Northern California ( This AMI has wwbank demo pre-installed )
AMI_ID=ami-043646c1ef78a684d
AWS_REGION=us-west-1

###############################################################################
#  No edits required below
###############################################################################

# Username to SSH to instances. All of the pre-selected AMIs use the 'centos' user.
# If you use your own AMI, you may need to change the ssh username as well.
SSH_USERNAME=centos

# set component flags.  helpful in case you need to restart a failed build or use alternate setup
setup_prereqs=true
setup_onenode=true

```


### Export your AWS credentials


```
export AWS_ACCESS_KEY_ID=<your key>
export AWS_SECRET_ACCESS_KEY=<your secret key>
export AWS_DEFAULT_REGION=<aws region you want to run in>

```

### Start the build:


```
#  run the setup
cd /app/app_name

. bin/utilities/setup.sh aws

```


### Sample output of a successful build


```


          ---------------------------------------------------------------------------------------------------------------------------------------------
          ---------------------------------------------------------------------------------------------------------------------------------------------
          |                  	Bind Mount -- SSH Connection String:                                                                                                            
          ---------------------------------------------------------------------------------------------------------------------------------------------
          ---------------------------------------------------------------------------------------------------------------------------------------------
          |       ssh -o StrictHostKeyChecking=no -o IdentitiesOnly=yes -o UserKnownHostsFile=/dev/null -i ~/fishermans_wharf/tlepple-us-west-1-i-0e1cec3878da09f63-10.0.8.81.pem centos@52.52.99.73          
          ---------------------------------------------------------------------------------------------------------------------------------------------
          ---------------------------------------------------------------------------------------------------------------------------------------------
```

---
---

### Usefull docker commands

```
# list all containers on host
docker ps -a

#  start an existing container
docker start aws_build_cnt

# connect to command line of this container
docker exec -it aws_build_cnt bash

#list running container
docker container ls -all

# stop a running container
docker container stop aws_build_cnt

# remove a docker container
docker container rm aws_build_cnt

# list docker volumes
docker volume ls

# remove a docker volume
docker volume rm app_name_vol1
```
---
---

# To clean up and terminate all items run

```
cd /app/app_name

. bin/provider/aws/terminate_everything.sh

```
