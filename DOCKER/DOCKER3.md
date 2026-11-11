
### Instalacion de portainer en docker que esta en debian 13
```
Comando de instalación 
docker run -d \
  -p 8000:8000 \
  -p 9443:9443 \
  --name portainer \
  --restart=always \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v portainer_data:/data \
  portainer/portainer-ce:lts
REVISAR LOS LOGS para buscar el token 
docker logs portainer
	setup_token=8c5d2d05dfeec4788ee1bca8730de798ad50980e45e2491af8c487606a3047a8
```
