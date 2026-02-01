# MLOps-Docker-Workflow

A comprehensive repository for Docker practice and demonstration in MLOps course. This repository contains practical examples, detailed notes, and demonstrations of Docker concepts for containerizing applications.

---

## 📚 Table of Contents

- [Introduction to Docker](#introduction-to-docker)
- [Repository Structure](#repository-structure)
- [Docker Benefits](#docker-benefits)
- [Docker Architecture](#docker-architecture)
- [Docker Components](#docker-components)
- [Code Demonstration](#code-demonstration)
- [Docker Commands Reference](#docker-commands-reference)
- [Getting Started](#getting-started)
- [Additional Resources](#additional-resources)

---

## 🐳 Introduction to Docker

Docker helps developers build, package, and deploy applications quickly and efficiently. It solves the problem of **"it works on my machine"** by creating a consistent environment across development, testing, and production.

### What is Docker?

Docker containers encapsulate everything an application needs - like code, libraries, and dependencies - ensuring it runs reliably on any system.

### Key Problem Solved

Docker creates a **consistent environment** across:
- Development
- Testing  
- Production

This eliminates environment-related bugs and dependency conflicts, making applications truly portable.

---

## 📁 Repository Structure

```
MLOps-DOCKER-WORKFLOW/
├── .gitignore
├── app.py                 # Flask application
├── dockerfile            # Docker configuration file
├── LICENSE
├── README.md
├── requirements.txt      # Python dependencies
└── theory.txt           # Additional theory notes
```

---

## ✨ Docker Benefits

Docker provides three major advantages:

### 1. **Portability**
- Run applications consistently across different environments
- Move containers seamlessly between development, testing, and production

### 2. **Isolation**
- Each container runs independently
- No conflicts between different applications or dependencies
- Clean separation of concerns

### 3. **Scalability**
- Easy to scale applications horizontally
- Spawn multiple container instances as needed
- Efficient resource utilization

---

## 🏗️ Docker Architecture

Docker follows a client-server architecture with three main components:

### Architecture Diagram

```
┌─────────────────────────────────────────────────────────┐
│                    Docker Client                        │
│                    (Docker CLI)                         │
└─────────────────────────────────────────────────────────┘
                           │
                           │ REST API
                           ▼
┌─────────────────────────────────────────────────────────┐
│                   Docker Daemon                         │
│                 (Background Service)                    │
│                                                         │
│  ┌──────────────────────────────────────────────────┐  │
│  │              Container Manager                    │  │
│  │  • Manages containers, images, networks, volumes │  │
│  │  • Listens for API requests                      │  │
│  │  • Performs requested actions                    │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### Components Interaction

1. **Docker CLI (Client)**: The tool you use to interact with Docker when you type commands like `docker build` or `docker run`
2. **Docker Daemon (Server)**: Background service running on the host machine that manages Docker objects
3. **REST API**: Communication bridge between client and daemon

---

## 🧩 Docker Components

### 1. **Docker Image**

A Docker Image is a lightweight, standalone, and executable package that includes everything needed to run a piece of software.

**Characteristics:**
- **Lightweight**: Minimal OS overhead
- **Standalone**: Contains all dependencies
- **Scalable**: Can be used to create multiple containers

**Components of a Docker Image:**

```
┌─────────────────────────────────────┐
│        Application Code             │
├─────────────────────────────────────┤
│      OS (e.g., Ubuntu, Alpine)      │
├─────────────────────────────────────┤
│    Python Runtime (~v3.8)           │
├─────────────────────────────────────┤
│    Virtual Environment (venv)       │
├─────────────────────────────────────┤
│    Dependencies (requirements.txt)  │
├─────────────────────────────────────┤
│         Compatible Versions         │
└─────────────────────────────────────┘
```

### 2. **Docker Container**

A Docker Container is a lightweight, isolated, and executable environment created from a Docker Image.

**Key Features:**
- **Isolation**: Runs independently without affecting other containers
- **Ephemeral**: Can be started, stopped, or destroyed easily
- **Lightweight**: Shares host OS kernel (much lighter than VMs)

**Container vs Image:**
- **Image**: The blueprint/template
- **Container**: The live instance of the image

### 3. **Docker Image Lifecycle**

Understanding how Docker manages images:

#### **Creation**
Build a new image using a Dockerfile on top of a base image or from a registry.

#### **Storage**
Images are stored locally in your Docker environment and can be pushed to remote registries like Docker Hub.

#### **Distribution**
Images can be shared on distributed registries, making them available to a public/private registry.

#### **Execution**
When you run a Docker image, a container is created. This container is the live instance of the image.

---

## 💻 Code Demonstration

This repository contains a simple Flask application containerized using Docker.

### Application Code (`app.py`)

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def index():
    return '''
        <html>
            <body>
                <form action="/greet" method="POST">
                    Enter your name: <input type="text" name="username">
                    <input type="submit" value="Submit">
                </form>
            </body>
        </html>
    '''

@app.route('/greet', methods=['POST'])
def greet():
    user_input = request.form['username']
    return f"Hello {user_input}, Welcome to this app for Docker Demonstration."

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

### Dockerfile

```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define environment variable
ENV FLASK_APP=app.py

# Run the Flask app
CMD ["flask", "run", "--host=0.0.0.0"]
```

### Key Dockerfile Components Explained

| Component | Description |
|-----------|-------------|
| **FROM** | Specifies the base image (Python 3.8 slim version) |
| **COPY or ADD** | Adds files from your host system into the image |
| **RUN** | Executes commands in the image, such as installing dependencies |
| **CMD or ENTRYPOINT** | Defines the command that runs when the container starts |
| **EXPOSE** | Specifies the port the container will listen on |

---

## 🚀 Docker Commands Reference

### Building and Running

#### 1. **Build Docker Image**
```bash
docker build -t flask-app mlops-docker-demo .
```
- `-t`: Tag the image with a name
- `.`: Build context (current directory)

#### 2. **Run the Container**
```bash
docker run -p 5000:5000 mlops-docker-demo
```
- `-p`: Port mapping (host:container)
- Maps port 5000 on host to port 5000 in container

**Note:** This command can run anywhere in the system after the image is built.

#### 3. **Tag Your Image**
```bash
docker tag mlops-docker-demo arnav020/mlops-docker-demo:latest
```
- Format: `docker tag <image-name> <username>/<repository>:<tag>`

#### 4. **Push Image to Docker Hub**
```bash
docker push arnav020/mlops-docker-demo:latest
```
- Uploads the image to Docker Hub for sharing

### Management Commands

#### 5. **Pull Image from Docker Hub**
```bash
docker pull arnav020/mlops-docker-demo:latest
```
- Downloads the image from Docker Hub

#### 6. **Run the Pulled Image**
```bash
docker run -p 5000:5000 arnav020/mlops-docker-demo:latest
```

### Docker Image Workflow

```
┌──────────────┐         ┌──────────────┐
│   Foldable   │  ────>  │     Tent     │
│ Image (build)│         │Container(run)│
└──────────────┘         └──────────────┘
```

---

## 🎯 Getting Started

### Prerequisites

- Docker installed on your machine ([Download Docker](https://www.docker.com/products/docker-desktop))
- Basic understanding of Python and Flask
- Git for cloning the repository

### Step-by-Step Guide

1. **Clone the Repository**
   ```bash
   git clone https://github.com/yourusername/MLOps-Docker-Workflow.git
   cd MLOps-Docker-Workflow
   ```

2. **Build the Docker Image**
   ```bash
   docker build -t mlops-docker-demo .
   ```

3. **Run the Container**
   ```bash
   docker run -p 5000:5000 mlops-docker-demo
   ```

4. **Access the Application**
   - Open your browser and navigate to: `http://localhost:5000`
   - Enter your name and see the greeting message!

5. **Stop the Container**
   - Press `Ctrl + C` in the terminal
   - Or use `docker stop <container-id>`

---

## 📖 Docker Storage

Docker Storage is a lightweight, standalone, and scalable package that includes everything needed to run a piece of software. It is read-only and is used to create containers.

### Components of Docker Storage

#### 1. **Base Image**
The starting point, usually a minimal OS or runtime environment.

#### 2. **Layers**
Each change (like installing software, adding files) in the image adds a new layer. Docker uses a layered filesystem, so layers are stacked to form the final image.

#### 3. **Metadata**
Contains information like image size, creation date, and instructions on how to run it.

---

## 🔧 Theory

### Docker Daemon (Server)

The Docker Daemon is the **background service running on the host machine**. It is responsible for:

- Managing Docker objects like containers, images, networks, and volumes
- Listening for API requests
- Performing the actions you request

### Docker Engine

Docker Engine is the **core technology that powers Docker**, consisting of three main parts:

1. **Docker Daemon (Server)**: Background service managing Docker objects
2. **REST API**: Interface for communication between client and daemon
3. **Docker CLI (Client)**: Command-line tool for user interaction

---

## 📝 Additional Notes

### REST API in Docker

REST API allows communication between the Docker Daemon and the client. When you type commands like `docker build` or `docker run`, the CLI sends the command to the Docker Daemon via the REST API.

### Docker CLI (Client)

The CLI is the tool you use to interact with Docker. You type commands like `docker build`, `docker run`, etc.

### Dockerfile Instructions

The Dockerfile is a **text file with instructions** on how to build a Docker image. It's a blueprint for the image, specifying the environment, OS, and dependencies.

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 👨‍💻 Author

**Your Name**
- MLOps Course Practice Repository
- Docker Containerization Demonstration

---

## 🌟 Acknowledgments

- Docker Documentation
- Flask Framework
- MLOps Course Materials

---

**Happy Dockerizing! 🐳**