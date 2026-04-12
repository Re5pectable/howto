# Creating a new user

Related guides:
- [Install Docker](./install_docker.md)

## With password

```bash
export NEW_USER=rootless

sudo adduser --gecos "" "$NEW_USER"
```

## Without password

```bash
export NEW_USER=rootless

sudo adduser --disabled-password --gecos "" "$NEW_USER"
sudo passwd -l "$NEW_USER"
```

## Add to sudo (no password required on `sudo`)

```bash
export NEW_USER=rootless

echo "$NEW_USER ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/"$NEW_USER"
sudo chmod 0440 /etc/sudoers.d/"$NEW_USER"
sudo visudo -cf /etc/sudoers.d/"$NEW_USER"
```

## Add to sudo (password required on `sudo`)

```bash
export NEW_USER=rootless

echo "$NEW_USER ALL=(ALL:ALL) ALL" | sudo tee /etc/sudoers.d/"$NEW_USER"
sudo chmod 0440 /etc/sudoers.d/"$NEW_USER"
sudo visudo -cf /etc/sudoers.d/"$NEW_USER"
```

## SSH

```bash
su - "$NEW_USER"

mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

exit
```

Official docs:
- <https://help.ubuntu.com/community/AddUsersHowto>
- <https://manpages.ubuntu.com/manpages/latest/en/man8/visudo.8.html>
