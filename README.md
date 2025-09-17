 think1

 1. Opprett det delte NFS-volumet (hvis det ikke eksisterer)
docker volume create --driver local --opt type=nfs --opt o=addr=192.168.1.100,rw --opt device=:/srv/nfs/swarm-cache traefik-data

 2. Opprett Cloudflare-hemmeligheten
docker secret create cf_api_token ./cf_api_token.txt

 3. Opprett dashboard-hemmeligheten
docker secret create dashboard_credentials ./dashboard_credentials.txt

 4. Opprett Traefik-konfigurasjonen
docker config create traefik-config ./traefik.yml

docker network create --driver=overlay public-net

--------------------------- NFS server ---------------------------------------

sudo apt-get update
sudo apt-get install nfs-kernel-server

sudo mkdir -p /srv/nfs/swarm-cache

sudo chown nobody:nogroup /srv/nfs/swarm-cache
sudo chmod 777 /srv/nfs/swarm-cache

sudo nano /etc/exports

Add the following line at the bottom of the file. This tells the server to share the /srv/nfs/swarm-cache directory with any client on the network (*). For better security, you can replace * with your specific network range (e.g., 192.168.1.0/24).

/srv/nfs/swarm-cache    *(rw,sync,no_subtree_check)

Apply the changes you just made in the exports file and restart the NFS server.

sudo exportfs -a
sudo systemctl restart nfs-kernel-server

list delte mapper
showmount -e localhost
----------------------------------------------------------- tillat nfs uwf

sudo ufw allow from 192.168.1.0/24 to any port nfs




