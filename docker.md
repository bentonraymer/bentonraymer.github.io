# MacOS Instalation

1. Download Docker Desktop

[Docker: Accelerated Container Application Development](https://www.docker.com/)

1. Install with recommended options
2. Sign in to Docker account
3. Open terminal at bottom of Docker window
4. Paste Installation Command
    
    ```bash
    mkdir puter && cd puter && mkdir -p puter/config puter/data && sudo chown -R 1000:1000 puter && docker run --rm -p 4100:4100 -v `pwd`/puter/config:/etc/puter -v `pwd`/puter/data:/var/puter  ghcr.io/heyputer/puter
    ```
    
5. Go to ‘Images’
6. Click ‘Run’ under the ‘Puter’ image
    1. Under ‘Optional Settings’, specify a container name and port
7. Open web browser and navigate to [`http://puter.localhost:####/`](http://puter.localhost:3901/) , where #### is the port specified in step 7a

# Fedora Installation

*Did I complete the project and then realize “hey wait… we were probably intended to do this in our VM, not our local machine using a GUI”? Yes. Yes I did!*

1. Remove old conflicting packages
    
    ```bash
    sudo dnf remove docker \
    docker-client \
    docker-client-latest \
    docker-common \
    docker-latest \
    docker-latest-logrotate \
    docker-logrotate \
    docker-selinux \
    docker-engine-selinux \
    docker-engine
    ```
    
2. Set up repository
    
    `sudo dnf -y install dnf-plugins-core`
    
    `sudo dnf-3 config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo`
    
3. Install Docker Engine
    
    `sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`
    
4. Start Docker Engine
    
    `sudo systemctl enable --now docker`
    
5. Verify Installation
    
    `sudo docker run hello-world`
    
6. Paste Puter installation commands
    
    `mkdir puter && cd puter && mkdir -p puter/config puter/data && sudo chown -R 1000:1000 puter && docker run --rm -p 4100:4100 -v pwd/puter/config:/etc/puter -v pwd/puter/data:/var/puter  [ghcr.io/heyputer/puter](http://ghcr.io/heyputer/puter)`
    
7. Open Web Browser and navigate to `puter.localhost:4100`
