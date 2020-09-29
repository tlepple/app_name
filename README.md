# app_name
testing some modular items


# Build a docker container to execute this build


###  Setup some directories on mac for assets to be shared between docker container and MacOS


```
# create a directory in OS for bind mount:
cd ~
mkdir -p ./Documents/cloud_stuff/docker_stuff

# create a softlink to this directory
sudo ln -s /Users/$USER/Documents/cloud_stuff/docker_stuff ~/fishermans_wharf

```

Create the docker container and mount the directories


```
# Create a docker volume to persist data
docker volume create app_name_vol1

# Create the docker container
docker run -it \
  --name tim_bruce_tony_env \
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

### Export your AWS credentials


```
export AWS_ACCESS_KEY_ID=<your key>
export AWS_SECRET_ACCESS_KEY=<your secret key>
export AWS_DEFAULT_REGION=<aws region you want to run in>

```

### Edit the properties file 


```


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
