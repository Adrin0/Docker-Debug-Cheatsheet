## Pro Tips
* Use `docker exec -it <container> sh`to debug networking inside the container
* Prefer user-defined networks instead of the default `bridge`

---

### `issues/volumes.md`
```markdown
# Volume & Mount Issues

Persistent data is critical, but volumes can cause problems if misconfigured.  

---

## Symptoms
- Changes in files aren’t persisting
- “Permission denied” when writing to a mounted folder
- Host files not showing inside container

---

## Solutions

### 1. Correct Mount Syntax
```bash
docker run -v /host/path:/container/path myimage
```
* Ensure /host/path exists on the host.
* Relative paths can cause silent errors. Use absolute paths.

### 2. Fix Permission Problems
If container can't write:
```bash
ls -ld /host/path
chmod 777 /host/path   # quick test
```
Better: Match user IDs:
```bash
docker run -u $(id -u):$(id -g) -v /host/path:/data myimage
```
### 3. Named Volumes
```bash
docker volume create mydata
docker run -v mydata:/var/lib/mysql mysql
```

check:
```bash
docker volume inspect mydata
```