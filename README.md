# 🚀 Container Networking Lab: Balancing Isolation & Connectivity

## 📌 Objective
The goal of this lab exercise is to explore and demonstrate **network isolation** within Docker containers.  
You’ll learn how containers inside the **same custom bridge network** can communicate seamlessly, while those on **different networks remain isolated** — a critical concept for securing containerized applications and microservices.

---

## 🌐 Introduction to Docker Networking
Docker networking enables **secure communication** between containers while maintaining **isolation**. It offers different types of networking modes:

### 🔹 Types of Docker Networks:
- **Bridge (Default)** – Connects containers internally on a single host.
- **Custom Bridge** – User-defined bridge networks with better control.
- **Host** – Containers share the host's network stack.
- **Overlay** – Enables communication across multiple hosts (Docker Swarm).
- **Macvlan** – Assigns a unique MAC address to each container.
- **None** – Completely disables networking.

🔍 In this lab, we focus on the **Custom Bridge Network** for controlled and secure communication.

---

## ⚡ Why Use a Custom Bridge Network?
A **custom bridge network** offers several advantages:
- ✅ **Security**: Containers on different networks are isolated by default.
- ✅ **Performance**: Direct internal communication without external routing.
- ✅ **Service Discovery**: DNS-based name resolution between containers.
- ✅ **Customization**: Ability to define IP ranges, subnets, and gateways.

---

## 🔧 1. Create a Custom Bridge Network
Run the following command to create a custom bridge network:

```bash
docker network create --driver bridge --subnet 172.20.0.0/16 --ip-range 172.20.240.0/20 prakriti-bridge
```

### 🔍 Explanation:
- `--driver bridge`: Uses the **bridge** network driver.
- `--subnet`: Specifies the network’s **address range**.
- `--ip-range`: Defines the **dynamic IP pool**.

---

## 🚀 2. Run Containers Inside the Custom Network
### Start a **Redis Container** (`prakriti-database`)
```bash
docker run -itd --net=prakriti-bridge --name=prakriti-database redis
```

### Start a **BusyBox Container** (`prakriti-server-A`)
```bash
docker run -itd --net=prakriti-bridge --name=prakriti-server-A busybox
```

### 📋 Check IP Addresses
Inspect the network and view container IPs:

```bash
docker network inspect prakriti-bridge
```

✅ Expected Output:
```
prakriti-database: 172.20.240.1
prakriti-server-A: 172.20.240.2
```

---

## 🔄 3. Test Communication Between Containers
### Ping from `prakriti-database` to `prakriti-server-A`:
```bash
docker exec -it prakriti-database ping prakriti-server-A
```

### Ping from `prakriti-server-A` to `prakriti-database`:
```bash
docker exec -it prakriti-server-A ping prakriti-database
```

✅ Expected Outcome: **Successful pings** indicate that containers within the same network can communicate.

---

## 🚧 4. Demonstrate Network Isolation
Now launch another container outside the custom network:

### Start **BusyBox** (`prakriti-server-B`) in the default `bridge` network:
```bash
docker run -itd --name=prakriti-server-B busybox
```

### 📋 Get the IP Address of `prakriti-server-B`:
```bash
docker inspect -format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' prakriti-server-B
```
(Example IP: `172.17.0.2`)

---

## ❌ 5. Test Cross-Network Communication
### Ping from `prakriti-database` to `prakriti-server-B`:
```bash
docker exec -it prakriti-database ping 172.17.0.2
```

🚨 **Expected Outcome:**  
The ping should **fail**, proving that containers on **different networks cannot communicate** by default.

---

## 🔍 6. Inspect Networks for Confirmation
To verify the network associations:

```bash
docker network inspect prakriti-bridge
docker network inspect bridge
```

✅ `prakriti-bridge` should contain `prakriti-database` and `prakriti-server-A`.  
✅ `bridge` should contain `prakriti-server-B`.

---

## 🏆 Conclusion
- 🔥 **Containers within the same network** can communicate directly.
- 🔒 **Containers across different networks** are **isolated by default**.
- 🎯 Docker’s **networking model** ensures secure and controlled communication.

🚀 **You now have a solid understanding of Docker Bridge Networking and container isolation!** 🎯

---

# 🛠 Bonus Tips:
- 📤 **To remove the custom network:**
  ```bash
  docker network rm prakriti-bridge
  ```
- 🔄 **To connect a container to multiple networks:**
  ```bash
  docker network connect <network-name> <container-name>
  ```

---

# 🌟 Final Words
Mastering container networking is **key** to building scalable, secure, and efficient microservices architectures.  
🔹 Continue exploring **Overlay Networks**, **Docker Compose**, and **advanced networking models** to enhance your container expertise!

🎨 **Happy Dockerizing, Prakriti!** 🚀

---

  
