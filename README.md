# zoomcamp
Based on the [mlops-zoomcamp](https://github.com/DataTalksClub/mlops-zoomcamp) course by **DataTalksClub**

# AWS Setup Web
1. Create a new account on [AWS](aws.amazon.com)
2. Create a new admin user in IAM, logout of root and log back in with new admin account
3. Create an EC2 instance (*t2.micro with 30GB space for testing and t2.large for actually running stuff*)
4. Create a key pair and dowload it on your machine
5. Go to the security group for the instance, edit inbound rules and add a rule to open SSH
6. Open TCP and UDP port 4200 for prefect as well as HTTP and HTTPS

# Local Setup
1. Dowload key from AWS and move it to your ~/.ssh folder
2. Change permissions of your .pem file and make it read only: `chmod 400 YOUR_KEY_NAME.pem`
3. Add a config file in your ~/.ssh folder with the following contents:
```host mlops-zoomcamp
    HostName THE_PUBLIC_IP_ADDRESS_OF_YOUR_AWS_EC2_INSTANCE
    User ubuntu
    IdentityFile FULL_PATH_TO_YOUR_PEM_FILE
    StrictHostKeyChecking no
```

## VS Code Setup
1. Install Remote - SSH extension
2. Install Jupyter extension on remote
3. Set full path to SSH executable in Settings->Remote.SSH: Path. Run `which ssh` to get the path. I had to do this on a 32bit Windows machine.
4. Configure port forwarding in VS Code and forward following ports:
  1. 8888 for Jupyter
  2. 5000 for MlFlow
  3. 4200 for Prefect

# AWS Terminal
1. Update apt repos: `sudo apt update`
2. Upgrade all packages: `sudo apt upgrade`
3. Add colour to bash prompt: `nano bashrc` and then uncomment `force_color_prompt=yes`
4. Check memory usage: `vmstat`
5. Check data usage: `df`
6. If you need to add a second key to access the EC2 instance (*from another machine, like a mobile device*) add the second RSA public key in the .ssh/authorized_keys file and reboot the EC2 instance

## Install Docker
1. Install Docker: `sudo apt install docker.io`
2. Create a folder for software: `mkdir soft`
3. Go to new folder: `cd soft`
4. To get the latest release of Docker Compose, go to https://github.com/docker/compose and download the release for your OS. Download Docker Compose: `wget https://github.com/docker/compose/releases/download/v2.6.0/docker-compose-linux-x86_64`
5. Make the script executable: `chmod +x docker-compose-linux-x86_64`
6. Rename the executable to a shorter version: `mv docker-compose-linux-x86_64 docker-compose`
7. Go back to home folder: `cd ~`
8. Add soft folder to path: `nano bashrc` then add `export PATH="${HOME}/soft:${PATH}"` save and exit
9. Reload bash config: `source .bashrc`
10. Run Docker Hello World: `sudo docker run hello-world`
11. Create a docker group if it doesn't exist: `sudo groupadd docker`
12. Add yourself to the docker group: `sudo usermod -aG docker $USER`
13. Run Docker without the need to sudo: `docker run hello-world`

## GitHub and Git Setup
1. Generate private and public key: `ssh-keygen -t ed25519 -C "YOUR_GITHUB_EMAIL"`
2. Move key to .ssh folder: `mv KEYNAME* .ssh/`
3. Start SSH Agent: `eval "$(ssh-agent -s)"`
4. Add key to agent:  `ssh-add ~/.ssh/KEYNAME`
5. Check which keys are added to agent: `ssh-add -l -E sha256`
6. If you would like to have the ssh-agent start automatically, follow this: [start ssh agent on login](https://stackoverflow.com/questions/18880024/start-ssh-agent-on-login)
```
# To start on login
if [ -z "$SSH_AUTH_SOCK" ]; then
    eval `$(ssh-agent -s)`
    ssh-add .ssh/KEYNAME
fi
# To stop on logout
trap 'test -n "$SSH_AUTH_SOCK" && eval `/usr/bin/ssh-agent -k`' 0
```
7. Get public key to copy paste on GitHub: ` cat ~/.ssh/KEYNAME.pub`
8. Go to *Settings->SSH and GPG keys* on GitHub (Web), click on *New SSH key* and paste the public key
9. Go to your repo on GitHub and Clone it using SSH
10. Go back to the terminal and to get code from GitHub run: `git clone YOUR_SSH_GITHUB_LINK`
11. Add your GitHub user to git config: `git config --global user.name "YOUR_GITHUB_USER"`
12. Add your email to git config: `git config --global user.email "YOUR_GITHUB_EMAIL"`
13. View git config settings: `git config -l`
14. If you forked and need to add the upstream repo: `git remote add upstream https://github.com/DataTalksClub/mlops-zoomcamp.git`
15. To see all your remotes: `git remote -v`
16. If you 

## Miniconda Setup
1. Get: `wget https://repo.anaconda.com/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh`
2. Install: `bash Miniconda3-py39_4.12.0-Linux-x86_64.sh`
3. Delete installation script: `rm Miniconda3-py39_4.12.0-Linux-x86_64.sh`
4. Update Conda: `conda update -n base -c defaults conda`
5. Create new conda environment: `conda create --name mlops`
6. Activate new environment: `conda activate mlops`

## Install Python and Libraries
1. Install latest Python version: `conda install python`
2. I had to separately install mlflow so: `conda install mlflow`
3. Similarly for xgboost: `conda install xgboost`
4. Install needed packages: `pip install -r requirements.txt` They will be in different folders, so either move to the needed folder or provide the path to it.
5. `pip install boto3`

## MlFlow
1. Start UI: `mlflow ui --backend-store-uri sqlite:///mlflow.db`
2. Start server: `mlflow server --backend-store-uri sqlite:///mlflow.db --default-artifact-root ./mlruns`
4. UI runs on port 5000: http://localhost:5000/

## Setup Prefect
1. Set public URL: `prefect config set PREFECT_ORION_UI_API_URL="YOUR_EC2_PUBLIC_ADDRESS/api"`
2. Might need to unset it: `prefect config unset PREFECT_ORION_UI_API_URL`
3. Start prefect: `prefect orion start --host 0.0.0.0`
4. UI runs on port 4200: http://localhost:4200











