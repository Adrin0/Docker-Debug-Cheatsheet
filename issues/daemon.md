## Pro Tips
* Use bind mounts for development, named volumes for production
* Inspect volumes with `docker system df -v`.

---

### `issues/daemon.md`
```markdown
# Docker Daemon Issues

If the Docker daemon isn’t running properly, nothing works.  

---

## Symptoms
- `docker: Cannot connect to the Docker daemon`
- Services fail to start
- Daemon crashes or stops unexpectedly

---

## Solutions

### 1. Check if Docker Daemon Is Running
```bash
systemctl status docker
```
If stopped:
```bash
sudo systemctl start docker
```

### 2. Enable Auto-Start
```bash
sudo systemctl enable docker
```
### 3. Check Logs
```bash
journalctl -u docker -f
```
### 4. Reset Docker
If corruption suspected:
```bash
sudo systemctl stop docker
sudo rm -rf /var/lib/docker
sudo systemctl start docker
```
* This deletes all containers/images.