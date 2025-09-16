# Networking Issues in Docker

Networking problems are some of the most common headaches when working with containers.  

---

## Symptoms
- Container can’t connect to the internet
- Host can’t reach the container (e.g., `localhost:8080` doesn’t respond)
- Containers can’t talk to each other
- DNS resolution fails inside container

---

## Solutions

### 1. Check Container Ports
```bash
docker ps
```
* Verify the ```ports``` column
* If you expected ```0.0.0.0:8080->80/tcp``` but don't see it, update your run command:
```bash
docker run -p 8080:80 myimage
```
### 2. Restart Docker Network
```bash 
docker network ls
docker network inspect bridge
docker network prune  # Removes unused networks
```
### 3. Fix DNS Issues
If containers can't resolve domain names:
```bash
docker run busybox nslookup google.com
```
* If it fails, add DNS to `etc/docker/daemon.json`
```json
{
  "dns": ["8.8.8.8", "8.8.4.4"]
}
```
Then restart Docker.
### 4. Containers Can't Talk to Eachother
* Ensure they're on the same network:
```bash
docker network create mynet
docker run -d --name web --network mynet nginx
docker run -it --network mynet busybox ping web
```

---

## Pro Tips
* Use `docker exec -it <container> sh`to debug networking inside the container
* Prefer user-defined networks instead of the default `bridge`