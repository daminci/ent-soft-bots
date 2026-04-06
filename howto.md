# How to Move Docker Images from Orion (Windows) to Apollo (Ubuntu)

Apollo runs a local Docker registry at `registry.apollo.local`.

---

## 1. Tag the image for the local registry

On **Orion** (Windows), tag your image with the registry prefix:

```bash
docker tag <image-name>:latest registry.apollo.local/<image-name>:latest
```

Example:
```bash
docker tag nginx-crash-course-app1:latest registry.apollo.local/nginx-crash-course-app1:latest
```

> Note: `docker tag` does not create a new image — it adds an alias pointing to the same image ID.

---

## 2. Log in to the local registry

```bash
docker login registry.apollo.local
```

Use your Apollo registry credentials when prompted.

---

## 3. Push the image to Apollo's registry

```bash
docker push registry.apollo.local/<image-name>:latest
```

Example:
```bash
docker push registry.apollo.local/nginx-crash-course-app1:latest
```

---

## 4. Pull the image on Apollo

SSH into **Apollo**, then pull the image:

```bash
docker pull registry.apollo.local/<image-name>:latest
```

---

## 5. Run the container on Apollo

```bash
docker run -d --name <container-name> -p <host-port>:80 registry.apollo.local/<image-name>:latest
```

Example (maps host port 8080 to container port 80):
```bash
docker run -d --name nginx-crash-course-app1 -p 8080:80 registry.apollo.local/nginx-crash-course-app1:latest
```

---

## Quick reference — all three apps

```bash
# On Orion: tag and push
docker tag nginx-crash-course-app1:latest registry.apollo.local/nginx-crash-course-app1:latest
docker tag nginx-crash-course-app2:latest registry.apollo.local/nginx-crash-course-app2:latest
docker tag nginx-crash-course-app3:latest registry.apollo.local/nginx-crash-course-app3:latest

docker push registry.apollo.local/nginx-crash-course-app1:latest
docker push registry.apollo.local/nginx-crash-course-app2:latest
docker push registry.apollo.local/nginx-crash-course-app3:latest

# On Apollo: pull and run
docker pull registry.apollo.local/nginx-crash-course-app1:latest
docker pull registry.apollo.local/nginx-crash-course-app2:latest
docker pull registry.apollo.local/nginx-crash-course-app3:latest

docker run -d --name nginx-crash-course-app1 -p 8080:80 registry.apollo.local/nginx-crash-course-app1:latest
docker run -d --name nginx-crash-course-app2 -p 8081:80 registry.apollo.local/nginx-crash-course-app2:latest
docker run -d --name nginx-crash-course-app3 -p 8082:80 registry.apollo.local/nginx-crash-course-app3:latest
```

---

## Notes

- The `:1.0` versioned tag push failed because the tag wasn't registered in the registry — always use the exact tag you pushed (`:latest` in this case).
- Layers already present in the registry are reused (`Mounted from ...`) — subsequent pushes of similar images are fast.
- Nginx on Apollo can reverse-proxy any running container via `proxy_pass http://localhost:<host-port>;`.
