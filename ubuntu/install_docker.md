# Install Docker on Ubuntu


Related guides:
- 

## Rootful

```bash
sudo apt-get update -y
sudo apt-get upgrade -y

sudo apt-get remove -y docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc

sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt-get update -y

sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras

sudo systemctl enable --now docker
sudo docker run --rm hello-world
```

## Rootless

```bash
sudo apt-get install -y uidmap dbus-user-session

sudo systemctl disable --now docker.service docker.socket
sudo rm -f /var/run/docker.sock
```

[Create a new user](./new_user.md)
```bash
sudo loginctl enable-linger [rootless_user]
exit
```

```bash
# reconnect via SSH as the new user

dockerd-rootless-setuptool.sh install
systemctl --user enable --now docker
docker ps
```
