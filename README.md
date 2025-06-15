````markdown
# 🧰 Java HTTP Server on Kubernetes 🚀

![Docker](https://img.shields.io/badge/Docker-Containerized-blue?logo=docker)
![Java](https://img.shields.io/badge/Java-17-blue.svg?logo=java)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Deployed-blue?logo=kubernetes)
![License](https://img.shields.io/badge/License-MIT-green)

This project demonstrates how to:

✅ Build a basic **Java HTTP Server**  
✅ Containerize it using **Docker**  
✅ Deploy it on **Kubernetes using Minikube**  
✅ Expose it via a **NodePort service**

---

## 🗂️ Project Structure

```bash
java-k8s-project/
├── src/
│   └── Main.java              # Java server code
├── Dockerfile                 # Docker image build file
├── deployment.yml             # K8s Namespace, Deployment & Service
└── README.md                  # This file
````

---

## 🔧 Prerequisites

Before you begin, ensure the following tools are installed:

* ✅ [Java 17+](https://adoptopenjdk.net/)
* ✅ [Docker](https://docs.docker.com/get-docker/)
* ✅ [Minikube](https://minikube.sigs.k8s.io/)
* ✅ [kubectl](https://kubernetes.io/docs/tasks/tools/)

---

## 👨‍💻 Java Server Code

Simple server using `com.sun.net.httpserver.HttpServer`:

```java
HttpServer server = HttpServer.create(new InetSocketAddress(8080), 0);
server.createContext("/", new MyHandler());
server.setExecutor(null);
server.start();
```

Responds with:

```html
<h1>✅ Hello from Java on Kubernetes!</h1>
```

---

## 🐳 Dockerization

### Dockerfile

```Dockerfile
FROM openjdk:17
WORKDIR /app
COPY src/Main.java .
RUN javac Main.java
EXPOSE 8080
CMD ["java", "Main"]
```

### Build & Push Image

```bash
# Build
docker build -t vignesh342/java-k8s-project .

# Push to Docker Hub
docker push vignesh342/java-k8s-project
```

---

## ☸️ Kubernetes Deployment

### Apply K8s Manifest

```yaml
# deployment.yml

apiVersion: v1
kind: Namespace
metadata:
  name: java-k8s-ns
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: java-k8s-app
  namespace: java-k8s-ns
spec:
  replicas: 1
  selector:
    matchLabels:
      app: java-k8s-app
  template:
    metadata:
      labels:
        app: java-k8s-app
    spec:
      containers:
      - name: java-k8s-app
        image: vignesh342/java-k8s-project
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: java-k8s-service
  namespace: java-k8s-ns
spec:
  type: NodePort
  selector:
    app: java-k8s-app
  ports:
    - port: 80
      targetPort: 8080
      nodePort: 30080
```

### Deploy Resources

```bash
kubectl apply -f deployment.yml
```

---

## 🌍 Access the Application

### Method 1: Using Minikube Service (Recommended)

```bash
minikube service java-k8s-service -n java-k8s-ns
```

This opens the app in your browser.
You should see: ✅ `Hello from Java on Kubernetes!`

### Method 2: Manually via NodePort

```bash
minikube ip  # Get Minikube IP
```

Then open: `http://<MINIKUBE_IP>:30080`

---

## 🧪 Validate Everything is Working

```bash
# Check all components
kubectl get all -n java-k8s-ns
```

You should see 1 pod, 1 service, and 1 deployment running.

---

## 🧹 Cleanup

```bash
kubectl delete -f deployment.yml
```

---

## 📸 Screenshots

> ✅ Add a screenshot of the webpage here if needed
> (e.g., browser showing "Hello from Java on Kubernetes!")

---

## 🧠 Learning Outcomes

* 🚀 Java server creation with built-in `HttpServer`
* 🐳 Dockerizing and publishing to Docker Hub
* ☸️ Kubernetes Deployment & Namespace isolation
* 🔗 Exposing apps using NodePort and Minikube tunneling

---

## 👨‍💻 Author

**Vignesh Sadanki**
📎 [GitHub](https://github.com/Sadanki)

---
