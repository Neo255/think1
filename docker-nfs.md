 Rens opp gamle volumes først hvis de har vært installert før
docker volume rm traefik-data cloudflare-data poolserver-data

 Opprett nye med riktige stier

docker volume create --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.40.200,rw \
  --opt device=:/srv/nfs/copi-pool/traefik \
  traefik-data

docker volume create --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.40.200,rw \
  --opt device=:/srv/nfs/copi-pool/cloudflare \
  cloudflare-data

docker volume create --driver local \
  --opt type=nfs \
  --opt o=addr=192.168.40.200,rw \
  --opt device=:/srv/nfs/copi-pool/poolserver \
  poolserver-data
