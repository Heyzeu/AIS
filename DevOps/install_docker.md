````bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable --now docker

##Vérif si ça fonctionne
docker --version
docker run hello-world

#Install Portainer
docker volume create portainer_data
docker run -d -p 9000:9000 --name=portainer --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data portainer/portainer-ce
````
 Ensuite, accéder à http://IP_DE_LA_VM:9000 depuis un navigateur.