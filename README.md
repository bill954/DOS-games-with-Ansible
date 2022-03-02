# My first Ansible Project: DOS games in docker containers

This project consists of an ansible playbook that sets up two (or more) servers to work as:

[web_server]
Hosts a main web page that works as a launcher for the dockerized DOS games.

[games_server]
Runs the containers for each DOS game.

# Instructions:
- Before starting, you should have at least other two computers or virtual machines to work as servers. If you have these you can write their ip's in the inventory file, replacing the ones that are already written. This playbook was written using a Manjaro distro in the [games_server] group and Ubuntu in the [web_server] group.
Each server must have the same user and password and I recommend to match their username with your main pc username.

- Once you have your external terminals or virtual machines set, the first thing you need to do is establish a ssh connection between and your main terminal them using a ssh generated key.
  - First generate a ssh key:
```
ssh-keygen -t ed25519 -C "ansible key"
```
  - When asked in which file it should be saved, write: ansible_key. Ssh also gives you the option to add a passphrase to the key you can press enter and leave it empty to save time.

  - Then copy the public key to the server. Repeat this step for each server you want to use. Add -p and the port number only when needed.

```
ssh-copy-id -i ~/.ssh/ansible_key.pub your_server_ip -p your_port
```

- Modify the main web page html file replacing the word localhost with the ip of the host that will work as your games_server:
```
$: sed -i 's/localhost/your_games_server_ip/g' roles/web_server/files/index.html
```
  - If you want this to be accesible to people outside of your local network, use your public ip in this step. Later you should enable port forwarding from your router to the ports 81 to 86 of the games server and the port 8080 of your router to your web server (or more ports if you add other servers).

Then, to run the playbook and set the servers just run this command and enter your password:
```
ansible-playbook --ask-become-pass dos_docker_games.yml
```

