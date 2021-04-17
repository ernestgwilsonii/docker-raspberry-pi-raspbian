## Install docker and docker-compose on a Raspberry Pi running a Raspian OS image

### SSH log-in to your Raspberry Pi as user "pi"
```
ssh pi@ipAddressOfYourPi
# raspberry is the default password for username pi (until you change it and please do!)
#sudo bash -

# Install Docker
sudo curl -fsSL get.docker.com -o get-docker.sh && sudo sh get-docker.sh

# Option A (New style):
# Run Docker as a non-privileged user, consider setting up the Docker daemon in rootless mode for your user:
# REF: https://docs.docker.com/go/rootless/ 
#sudo dockerd-rootless-setuptool.sh install

# Option B (Typical)
# Run the Docker daemon as a fully privileged service, but granting non-root users access
# REF: https://docs.docker.com/go/daemon-access/
# WARNING: Access to the remote API on a privileged Docker daemon is equivalent to root access on the host
# REF: https://docs.docker.com/go/attack-surface/

# Allow the current logged user (probably "pi") to run Docker without typing sudo in the future
sudo groupadd docker # If the group already exists, that is ok
sudo gpasswd -a $USER docker
sudo gpasswd -a pi docker
sudo usermod -a -G docker $USER
sudo usermod -a -G docker pi
newgrp docker
sudo -u pi newgrp docker

# Optional: Add other users (If you have another Linux login user named "bob" that also needs to run Docker)
#sudo usermod -aG docker bob

# Verify that you can now run Docker!
which docker # /usr/bin/docker
docker --version
docker run hello-world


# Install docker-compose
sudo apt update -y
sudo apt install -y python3-pip libffi-dev
sudo pip3 install docker-compose

# Verify
which docker-compose # /usr/local/bin/docker-compose
docker-compose --version


# Alternate method
#sudo bash -
#cd /tmp
#git clone https://github.com/docker/compose.git
#cd compose
#git checkout release
# Build it
#time docker build -t docker-compose:armhf -f Dockerfile.armhf .
# Create the binary
#time docker run --rm --entrypoint="script/build/linux-entrypoint" -v $(pwd)/dist:/code/dist -v $(pwd)/.git:/code/.git "docker-compose:armhf"
# Copy the binary into place
#sudo cp dist/docker-compose-Linux-armv7l /usr/local/bin/docker-compose
#sudo chown root:root /usr/local/bin/docker-compose
#sudo chmod 0755 /usr/local/bin/docker-compose
#/usr/local/bin/docker-compose --version
# Clean up
#cd /tmp
#rm -Rf /tmp/compose
#sudo docker system prune -af
# Verify
#which docker-compose # /usr/local/bin/docker-compose
#docker-compose --version


# Reboot the Raspberry Pi
sudo reboot

# SSH log-in to your Raspberry Pi as user "pi"
ssh pi@ipAddressOfYourPi

# Verify that Docker started automatically and that commands work!
vcgencmd measure_temp
docker --version
docker-compose --version
docker run hello-world # Should not need sudo to work!
```

### Create or join a Docker Swarm to use Docker Compose (future)
```
# REF: https://docs.docker.com/engine/swarm/swarm-tutorial/create-swarm/
docker swarm init
# To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions
```
