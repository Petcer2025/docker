---

# 📦 Docker Persistent Storage

### **Understanding Docker Volumes & Bind Mounts (Theory + Practical Guide)**

---

## 🧩 **1. Introduction**

By default, Docker containers are **ephemeral** — meaning any data written inside the container is lost when the container stops, restarts, or is deleted.

To preserve data, Docker provides two types of persistent storage:

1. **Bind Mounts**
2. **Volumes**

This README explains both with theory + practical commands.

---

## 🧱 **2. Why Persistent Storage?**

Containers delete all internal data when they stop.
Persistent storage is required for:

* Databases (MySQL, MongoDB, PostgreSQL)
* Logs
* Uploaded files
* Application data
* Configs

---

## 🗂 **3. Bind Mounts**

Bind Mounts map a **host machine directory** to a location inside a container.

```
Host Folder <--> Container Folder
```

### ✔ Ideal for:

* Development
* Live code editing
* Sharing config files

### ❌ Not ideal for production

(Host file permissions & security risks)

---

### **3.1 Bind Mount – Syntax**

```sh
docker run -v /path/on/host:/path/in/container image_name
```

### **3.2 Example (Linux/Mac)**

```sh
docker run -d \
  -v $(pwd)/data:/app/data \
  nginx
```

### **3.3 Example (Windows)**

```sh
docker run -d \
  -v C:\Users\Nash\data:/app/data nginx
```

---

### **3.4 Practical Test**

1. Create file inside container

```sh
docker exec -it <id> touch /app/data/test.txt
```

2. Check host folder — file exists.

---

## 📦 **4. Docker Volumes**

Volumes are storage spaces **created and managed by Docker**.

Docker controls everything → safe for production.

### ✔ Ideal for:

* Databases
* Production deployments
* Isolated & secure storage
* Backups

---

### **4.1 Create a volume**

```sh
docker volume create mydata
```

### **4.2 Use volume with container**

```sh
docker run -d \
  -v mydata:/app/data \
  nginx
```

### **4.3 List volumes**

```sh
docker volume ls
```

### **4.4 Inspect volume**

```sh
docker volume inspect mydata
```

### **4.5 Where does Docker store volumes?**

```
/var/lib/docker/volumes/<volume_name>/_data
```

---

### **4.6 Volume Persistence Test**

1. Create file

```sh
docker exec -it <id> sh
echo "hello" > /app/data/a.txt
```

2. Delete the container

```sh
docker rm -f <id>
```

3. Run a new container

```sh
docker run -it -v mydata:/app/data busybox sh
```

4. Check file

```sh
ls /app/data
```

✔ File still exists → persistent!

---

## 🆚 **5. Volumes vs Bind Mounts (Comparison)**

| Feature          | **Volumes**               | **Bind Mounts**  |
| ---------------- | ------------------------- | ---------------- |
| Managed By       | Docker                    | Host OS          |
| Best For         | Production                | Development      |
| Storage Location | `/var/lib/docker/volumes` | Anywhere on host |
| Security         | High                      | Low              |
| Performance      | Fast                      | Slightly slower  |
| Backup           | Easy                      | Manual           |

---

## 🧠 **6. When to Use What**

### **Use Volumes for:**

* MySQL, MongoDB, PostgreSQL
* Production services
* Long-term persistence
* When security matters

### **Use Bind Mounts for:**

* Development
* Editing files locally
* VS Code live changes
* Quick testing

---

## 💬 **7. Common Interview Answers**

### **What is a Docker Volume?**

> A Docker volume is persistent storage managed by Docker and stored outside the container lifecycle.

### **What is a Bind Mount?**

> A bind mount maps a directory from the host system into the container.

### **Why do containers need persistent storage?**

> Because container data is deleted when the container stops.

### **Where are volumes stored?**

> `/var/lib/docker/volumes/`

---

## 🛠 **8. Useful Commands Cheat Sheet**

```sh
# Create volume
docker volume create mydata

# Inspect volume
docker volume inspect mydata

# Remove volume
docker volume rm mydata

# Bind mount example
docker run -v $(pwd)/data:/app/data nginx

# Volume example
docker run -v mydata:/app/data nginx

# Enter container
docker exec -it <id> sh
```

---

## 📘 **9. Summary**

* **Bind Mounts** = Host folder connected to container
* **Volumes** = Docker-managed persistent storage

Volumes → Production
Bind Mounts → Development

---

