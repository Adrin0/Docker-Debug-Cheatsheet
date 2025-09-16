## Pro Tips 
* On macOS/Windows, restart Docker Desktop.
* Verify socket permissions `var/run/docker.sock` should  be `srw-rw----root:docker`.

---

### `issues/permissions.md`
```markdown
#  Permission Errors in Docker

One of the most common errors is **“Permission denied”** when accessing files or the Docker socket.  

---

## Symptoms
- `permission denied` when mounting volumes
- Can’t run `docker` without `sudo`
- Containers can’t access mounted files

---

## Solutions

### 1. Add User to Docker Group
```bash
sudo usermod -aG docker $USER
newgrp docker
```
### 2. Fix File Ownership
If container user differs:
```bash
chown -R 1000:1000 /data
```
Or run container with matching UID:
```bash
docker run -u $(id -u):$(id -g) ...
```
### 3. SELinux / AppArmor Issues
* On SELinux:
```bash
docker run -v /host:/container:Z myimage
```
* AppArmor can block mounts. Disable for testing:
```bash
docker run --security-opt apparmor=unconfined myimage
```