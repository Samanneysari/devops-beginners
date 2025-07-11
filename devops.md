# Devops

Topic of this Course:

* 1-http/https

* 2- Nginx

* 3-Docker

* 4-Git/Gitlab CI

* 5-Bash scripting

* 6-Ansible

* 7-Prometheus/Grafana

## http, https, Nginx

### What is HTTP?
HTTP, or Hypertext Transfer Protocol, is the foundation of data communication on the World Wide Web. It’s an application-layer protocol used for transmitting hypermedia documents, like HTML, between clients (like your browser) and servers. Basically, it’s how your computer asks for and gets web pages.
* Downside: It’s not secure—data can be intercepted and read by anyone.

### What is HTTPS?
HTTPS, or Hypertext Transfer Protocol Secure, is HTTP with a security upgrade. It uses SSL/TLS encryption to protect the data being sent. This means your info (like passwords or credit card numbers) stays private and safe.

* Upside: It’s secure, trusted, and shows a padlock in your browser.

### HTTP vs. HTTPS: Key Differences

| Feature         | HTTP (Hypertext Transfer Protocol) | HTTPS (Hypertext Transfer Protocol Secure) |
|---------------|--------------------------------|----------------------------------|
| **Security**  | Not secure                   | Secure                           |
| **Encryption** | No (data is plain)           | Yes (data is locked)             |
| **Padlock Icon** | No (browser warning possible) | Yes (shows trust)               |
| **Best For**  | Basic, non-sensitive pages    | Logins, payments, personal info |

### Cookie

A small piece of data stored in your browser by a website. It helps the website "remember" you (e.g., login status, preferences).
* Track user sessions (e.g., keeping you logged in).
* Store temporary data (e.g., items in a shopping cart).

**Example**:
When you log into a website, it might save a cookie like session_id=abc123 to identify you on future visits.

#### Why cookie matters in DevOps:

* Used in web apps for authentication and session management.
* Critical for testing user workflows (e.g., simulating logged-in users).

### Header
Metadata (extra information) sent with HTTP requests/responses. It’s like a "label" on a package that tells the server or client how to handle the data.

* Define content type (e.g., JSON, HTML).
* Authenticate requests (e.g., API keys).
* Control caching or security settings.

**Example** :

* Request header: Content-Type: application/json tells the server the data is in JSON format.
* Response header: Status: 200 OK confirms a successful request.
#### Why Header matters in DevOps :

* Headers ensure APIs and services communicate correctly.
* Used for security (e.g., adding Authorization: Bearer <token>).

### Body

The main content of an HTTP request or response. It’s the "payload" sent between a client and server.

* Send data to a server (e.g., form submissions, file uploads).
* Receive data from a server (e.g., API responses).

**Example** :
When creating a user via an API, the body might look like:

#### Why Body matters in DevOps :

* Contains the actual data being processed in APIs/microservices.
* Essential for testing and debugging workflows.
### Payload

The core data being transmitted in a request/response body. It’s the "meat" of the message.

* Transfer critical information (e.g., user input, sensor data).
* Used in integrations between systems (e.g., triggering a deployment).

**Example** :
A webhook payload sent to a CI/CD tool:

### 7 core HTTP request methods
#### 1. GET

Retrieve data from a server (e.g., loading a webpage or fetching API data).

* Read-only operation (no changes to data).
* Used for queries like "show me user details" or "list all files."

**Example** :
GET /api/users/123 (fetch details for user ID 123).

##### Why get matters in DevOps :

* Used for monitoring, health checks, or fetching logs/metrics.
* Idempotent (multiple identical requests = same result).
#### 2. POST

Send data to a server to create or update a resource (e.g., submitting a form or uploading a file).
* Create new data (e.g., a user account).
* Trigger actions (e.g., start a CI/CD pipeline).

**Example** :
POST /api/users with a body like {"name": "Alice"}.

##### Why post matters in DevOps :

* Used to trigger deployments, send alerts, or submit configuration changes.
* Not idempotent (multiple requests may create duplicates).
#### 3. PUT

* Replace an existing resource entirely or create it if it doesn’t exist.
* Full updates (e.g., overwrite a user’s profile).

**Example** :
PUT /api/users/123 with {"name": "Bob", "email": "bob@example.com"}.

##### Why put matters in DevOps :

* Idempotent: Safe to retry (e.g., updating infrastructure config).
* Used in automated scripts to enforce desired states.


#### 4. DELETE

* Remove a resource from the server.
* Delete data (e.g., a file, user account, or API key).

**Example** :
DELETE /api/users/123 (delete user ID 123).

##### Why delete matters in DevOps :

* Clean up old resources (e.g., stale containers or logs).
* Idempotent: Deleting a nonexistent resource = no error.

#### 5. PATCH

* Partially update a resource (unlike PUT, which replaces it).
* Modify specific fields (e.g., update a user’s email without changing other details).

**Example** :
PATCH /api/users/123 with {"email": "new@example.com"}.

##### Why patch matters in DevOps :

* Efficient for small updates (e.g., tweaking config files).
* Not always idempotent (depends on implementation).
#### 6. HEAD

* Same as GET, but only retrieve headers (no response body).
* Check metadata (e.g., "Does this resource exist?" or "When was it last modified?").

**Example** :
HEAD /api/users/123 returns headers like Status: 200 OK but no user data.

##### Why Head matters in DevOps :

* Validate API endpoints or check server health without transferring data.
* Used in monitoring tools to test connectivity.
#### 7. OPTIONS

* Ask the server which HTTP methods are allowed for a resource.
* Discover API capabilities (e.g., "Can I POST to this endpoint?").
**Example** :
OPTIONS /api/users might return Allow: GET, POST, PUT.

##### Why option matters in DevOps :

* Debug APIs or configure CORS (Cross-Origin Resource Sharing) policies.
* Verify permissions before performing actions.

### What Are HTTP Status Codes?

HTTP status codes are three-digit numbers that a server sends to your browser (or any client) to tell you what happened with your request. They’re split into five categories:

* **1xx (Informational)**: The server is working on something (not errors, but included for context).
* **2xx (Success)**: Your request worked (not errors, but good to know).
* **3xx (Redirection)**: You’re being sent somewhere else (some are relevant to your question).
* **4xx (Client Errors)**: You made a mistake in the request.
* **5xx (Server Errors)**: The server failed.

#### Common HTTP Error Codes
##### 4xx: Client Errors

* 1. **`400 Bad Request`**
      The server can’t figure out what you’re asking for because the request is messed up (e.g., bad formatting).
* 2. **`401 Unauthorized`**
      You need to log in or provide credentials to access this. It’s different from 403—here, you haven’t even proven who you are yet.
* 3. **`403 Forbidden`**
      (Covered above) You’re authenticated, but still not allowed.
* 4. **`404 Not Found`**
      The page or resource you’re looking for doesn’t exist—like a dead link.
* 5. **`405 Method Not Allowed`**
      You tried the wrong method (e.g., using POST when only GET is allowed).
##### 5xx: Server Errors
These mean the server screwed up, not you.

* 1. **`500 Internal Server Error`**
A catch-all error when something goes wrong on the server, but it won’t tell you what.
* 2. **`502 Bad Gateway`**
The server was talking to another server and got a bad response—like a middleman failing.
* 3. **`503 Service Unavailable`**
The server is down temporarily, maybe for maintenance or overload.
* 4. **`504 Gateway Timeout`**
The server took too long to get a response from another server it depends on.

#### 1xx: Informational
These codes tell you the server is working on your request. They’re not errors, and you usually don’t see them as a user.

* 1. **`100 Continue`**
The server has received part of your request and is waiting for you to send the rest. For example, this might happen when uploading a large file.
* 2. **`101 Switching Protocols`**
The server is switching to a different protocol, like moving from HTTP to WebSocket for real-time features (e.g., a chat app).

#### 2xx: Success
These codes mean your request worked, and there’s no error. They’re common when things go smoothly.

* 1. **`200 OK`**
The server got your request and sent back what you asked for, like a webpage or file.
* 2. **`201 Created`**
You successfully made something new on the server, like signing up for an account.
* 3. **`204 No Content`**
The server completed your request but has nothing to send back. For example, after deleting a file, you might get this.

#### 3xx: Redirection
These codes mean the server is redirecting you to a different place. They’re not errors, but they change where you end up.

* 1. **`301 Moved Permanently`**
The resource you want has a new permanent address. For example, a website might redirect an old URL to a new one forever.
* 2. **`302 Found`**
The resource is temporarily at a different URL. You’ll be sent there for now, like during a site update.
* 3. **`304 Not Modified`**
The content hasn’t changed since your last visit, so the server skips sending it again. This saves time when browsing.



### What is an HTTP Redirect?
An HTTP redirect is a way for a server to tell a client (such as a web browser) that the requested resource has been moved to another location. When a user visits a URL, the server can respond with a redirect status code, instructing the browser to automatically go to a different URL.

#### There are two main types of redirects:

**`Temporary Redirects`** – The resource is temporarily moved, and the client should continue using the original URL in future requests.
**`Permanent Redirects`** – The resource has permanently moved to a new URL, and the client should update its records accordingly.

* Moving a website or webpage to a new URL.
* Redirecting HTTP traffic to HTTPS.
* Handling outdated URLs in a user-friendly way.
* Implementing load balancing by sending requests to different servers.
* Comparison Table of HTTP Redirects

| Status Code | Type               | Description                                     | Does It Change HTTP Method? |
|------------|------------------|-------------------------------------------------|-----------------------------|
| **302 (Found)** | Temporary Redirect | The resource is temporarily at a new URL. Browsers may change `POST` to `GET`. | **Yes** (May change `POST → GET`) |
| **307 (Temporary Redirect)** | Temporary Redirect | The resource is temporarily at a new URL, but the HTTP method must be preserved. | **No** (Preserves original method) |
| **301 (Moved Permanently)** | Permanent Redirect | The resource has permanently moved to a new URL. Browsers may change `POST` to `GET`. | **Yes** (May change `POST → GET`) |
| **308 (Permanent Redirect)** | Permanent Redirect | The resource has permanently moved, and the HTTP method must be preserved. | **No** (Preserves original method) |

## Infrastructure as Code (IaC)

Infrastructure as Code (IaC) is the practice of managing and provisioning infrastructure (like servers, networks, load balancers, and databases) through machine-readable definition files, rather than through manual configuration or interactive tools.

**`Terraform`**: A leading IaC tool used for provisioning infrastructure. It is declarative, meaning you define the desired end state of your infrastructure, and Terraform figures out how to get there. It is cloud-agnostic, working with AWS, Azure, GCP, and more.

**`Ansible`**: A tool often used for configuration management. It is procedural, meaning you define the steps to take to configure a server to a desired state. It is excellent for installing software, updating configurations, and managing existing servers.

## Monitoring and Logging

Monitoring: The process of collecting and analyzing quantitative data (metrics) about a system's performance. It helps you understand the health of your system in real-time.

**`Prometheus`**: A popular open-source tool for collecting time-series metrics.

**`Grafana`**: A tool for creating dashboards to visualize the metrics collected by Prometheus.

**`Logging`**: The process of recording discrete events that happen in your application or system. Logs are essential for debugging and understanding what went wrong after an error occurs.

## Cloud-Native Concepts & Kubernetes
Cloud-native is an approach to building and running applications that fully leverages the advantages of the cloud computing model. Key principles include containers, microservices, dynamic orchestration, and automation.

**`Kubernetes (K8s)`** is the industry-standard open-source platform for container orchestration. While Docker Swarm is excellent for simpler applications, Kubernetes is designed to manage complex, large-scale, and highly available containerized applications across clusters of machines. It handles service discovery, load balancing, self-healing, and automated rollouts and rollbacks.

## Backup and Disaster Recovery (DR)

**`Backup`**: The process of creating a copy of your data so it can be restored in case of data loss. For stateful applications like databases, regular, automated backups are non-negotiable. This applies to data stored in Docker persistent volumes as well.

**`Disaster Recovery (DR)`**: This is the high-level plan and process for how to use your backups to quickly recover your application and business operations after a catastrophic failure (like a server crash or a data center outage). A good DR plan is tested regularly to ensure it works when needed.

## Docker
### What is Docker?
Docker is a tool that makes it super easy to create, run, and share applications by packaging them into small, portable units called containers. Think of Docker as a way to put your application (like a website, a game, or a program) and everything it needs—like code, libraries, settings, and tools—into a single "box" that can run anywhere, whether it’s your laptop, a server, or the cloud. It’s like a shipping container for software: standardized, lightweight, and ready to go!
Docker was first released in 2013 by a company called Docker, Inc., and it’s become hugely popular because it solves a big problem: "It works on my machine, but not on yours." With Docker, you avoid those headaches because the container ensures everything works the same way, no matter where it runs.

### What is a Container?
A container is a lightweight, standalone package that holds your application and all its dependencies—like the operating system bits it needs, libraries, and configuration files. Imagine a container as a tiny virtual computer that only has what your app needs to run, nothing extra. It’s not a full virtual machine (VM), though—it’s much smaller and faster because it shares the host computer’s operating system (like Windows, macOS, or Linux) instead of running a whole separate OS.

For example:

If you have a Python app, the container might include Python, your code, and any Python libraries you’re using.
You can run this container on your laptop, then move it to a server, and it’ll work exactly the same without any setup hassles.
Containers are created from images (more on that below), and Docker manages them for you.

### Key Pieces of Docker
Let’s break down the main parts of Docker so you can see how it all fits together:

#### 1. Docker Engine:
This is the core of Docker—the program that runs on your computer or server. It’s like the "manager" that builds, runs, and controls containers.
It has two main parts:
Docker Daemon: The background worker that does the heavy lifting (building containers, managing them, etc.).
Docker Client: The command-line tool (or GUI) you use to tell the daemon what to do, like “run this container” or “build this image.”
#### 2. Docker Image:
An image is like a blueprint or recipe for a container. It’s a read-only file that has your app, its dependencies, and instructions on how to run it.
Example: You might have an image for a web server with Nginx installed. You can make a container from that image, and it’ll start running Nginx.
Images are built using a file called a Dockerfile (more on that later).
#### 3. Docker Container:
A container is a running instance of an image. If an image is the recipe, the container is the cooked meal.
You can start, stop, or delete containers. They’re isolated from each other but share the same host OS, making them super efficient.
#### 4. Dockerfile:
This is a simple text file where you write instructions to build a Docker image. It’s like a script that says, “Install this software, copy these files, and run this command.”

#### 5. Docker Hub:
This is a big online library (like a cloud storage for Docker images) where people share pre-made images. Want a container with MySQL or Node.js? You can download an image from Docker Hub and start a container in seconds.
You can also upload your own images to share with others.

#### 6. Docker Registry:
A storage system for Docker images, allowing you to save, share, and distribute them.

Key details:

* Public Registries: Docker Hub is the most famous one, offering free public image hosting (e.g., docker pull nginx gets an image from Docker Hub).
* Private Registries: You can set up your own registry (e.g., on your server or cloud) for private images using Docker’s open-source registry software or services like Amazon ECR, Google Container Registry, or GitHub Container Registry.
Commands:
```
docker push: Upload an image to a registry.
```
```
docker pull: Download an image from a registry.
```
* Example: If you build an image called my-app, you might push it to a private registry at myregistry.com/my-app and pull it elsewhere later.
* Why it’s key: Without registries, sharing images would be a manual mess (like emailing zip files). Registries make Docker portable and collaborative.

| **Aspect**          | **Virtual Machine (VM)**                                      | **Container**                                                       |
|---------------------|-------------------------------------------------------------|---------------------------------------------------------------------|
| **Definition**      | A full, isolated computer simulated by software (hypervisor) with its own operating system (OS). | A lightweight, isolated package that runs an app and its dependencies, sharing the host OS. |
| **Operating System** | Includes a complete guest OS (e.g., Windows, Ubuntu) on top of the host OS. | Uses the host OS kernel; only includes app-specific libraries and binaries (no full OS). |
| **Size**           | Large (several GBs) due to the full OS and overhead.        | Small (MBs to a few GBs) since it’s just the app and minimal dependencies. |
| **Startup Time**    | Slow (minutes) because it boots an entire OS.              | Fast (seconds) since it only starts the app process.               |
| **Resource Usage**  | Heavy—each VM needs dedicated CPU, RAM, and storage for its OS. | Light—shares host OS resources, so it’s more efficient.            |
| **Isolation**       | Strong—fully isolated at the hardware level via hypervisor. | Good—isolated at the process level, but shares the kernel (slightly less secure). |
| **Portability**     | Less portable—depends on hypervisor compatibility (e.g., VMware, VirtualBox). | Highly portable—runs anywhere Docker is installed, regardless of the underlying system. |
| **Examples**        | VMware Workstation, VirtualBox, Hyper-V, KVM.              | Docker, Podman, containerd.                                        |
| **Use Case**        | Running multiple OSes or legacy apps needing a full environment. | Running microservices, modern apps, or dev/test environments efficiently. |
| **Storage**         | Persistent by default (VM has its own disk image).         | Ephemeral by default (data lost unless saved to volumes).         |
| **Networking**      | Each VM has its own virtual network stack (e.g., virtual NIC). | Containers share the host’s network stack but can have isolated namespaces. |
| **Setup Complexity** | Complex—requires configuring a full OS and resources.     | Simple—just define an image with a Dockerfile or pull from a registry. |
| **Performance**     | Slower due to OS overhead and hypervisor layer.            | Near-native performance since it runs directly on the host kernel. |
| **Management**      | Managed by hypervisors (e.g., vSphere, Hyper-V Manager).   | Managed by container engines (e.g., Docker Engine, Kubernetes).   |
| **Scalability**     | Harder to scale—spinning up new VMs takes time and resources. | Easy to scale—containers start quickly and use fewer resources.   |
| **Typical File**    | VM image file (e.g., .vmdk, .ova).                         | Container image (e.g., layered tarball in Docker).                |

installation guide:
```
sudo apt install docker.io
```
### Basic docker commands:
#### 1. Cheking docker status
```
systemctl status docker
```
#### 2. Docker version
Displays the Docker version information for both the client and server.
```
docker version
```
#### 3. Docker info
Provides detailed information about the Docker installation and system.
```
docker info
```
#### 4. Docker Images
Lists all images stored locally on your system.
```
docker images
```
#### 5. Docker pull
Downloads an image from a registry (e.g., Docker Hub) to your local machine.
```
docker pull docker.arvancloud.ir/grafana/grafana
```
#### 6. Docker run
Creates and starts a container from an image.
```
docker run -d -p 3000:3000 --name=grafana docker.arvancloud.ir/grafana/grafana
```
* **-d**: in the detached mode
* **-p**: port

#### 7. Docker ps
lists running containers

* Use -a to show all containers (running and stopped).
* Displays container ID, image, status, ports, and names.
```
docker ps
```
#### 8. Docker stop
Stops a running container.

* Sends a SIGTERM signal to the container’s main process, allowing it to shut down gracefully.
* Use the container ID or name.
```
docker stop nginx
```
#### 9.Docker start
Starts a stopped container.

* Restarts a container that was previously stopped, retaining its configuration (e.g., port mappings).
* Does not create a new container.
```
docker start nginx
```
#### 10. Docker restart
Restarts a running container.

* Equivalent to docker stop followed by docker start.
* Useful for refreshing a container’s state.
```
docker restart nginx
```
#### 11. Docker rm
Removes a stopped container.

* Use -f to force removal of a running container (stops it first).
* Frees up system resources by deleting unused containers.
```
docker rm nginx
```
#### 12. Docker rmi
Removes one or more images from your local system.

* Images must not be in use by any containers (running or stopped).
* Use -f to force removal if needed.
```
docker rmi nginx
```

| Command       | What it Removes | Example Target         | Notes                                   |
|---------------|------------------|-------------------------|-----------------------------------------|
| `docker rm`   | Containers        | `funny_container`       | Container must be **stopped** first     |
| `docker rmi`  | Images            | `ubuntu:20.04`          | No container should be using the image  |

#### 13. Docker exec

Runs a command inside a running container.

* Useful for debugging or interacting with a container’s processes.
* Use -it for an interactive terminal (e.g., a shell).
```
docker exec -it oraclelinux bash
```
#### 14. Docker logs
Fetches the logs of a container.

* Shows the output (stdout/stderr) of the container’s main process.
* Use -f to follow logs in real-time.
```
docker logs nginx
```
### Docker nexus
1. Download the Docker image using following commands.
```
docker pull sonatype/nexus
 ```
2. Build an image from a Nexus Dockerfile# docker build –rm –tag sonatype/nexus oss/
```
docker build –rm –tag sonatype/nexus-pro pro/ (For Pro)
```
 
3. To run (if port 8081 is open on your host):
```
docker run -d -p 8081:8081 –name nexus sonatype/nexus:oss
```
or to assign a random port that maps to port 8081 on the container:
```
docker run -d -p 8081 –name nexus sonatype/nexus
 ```

4. To determine the port that the container is listening on:
```
docker ps nexus
 ```

5. To test:
```
curl http://localhost:8081/service/local/status
 ```

6. To go into the container
```
docker exec -it <containerid> bash
```
7. admin password
```
cat /nexus-data/admin.password
```
8. example
```
sudo docker run -d --name nexus12 -p 8081:8081 -p 8082:8082 -v nexus-data:/nexus-data docker.arvancloud.ir/sonatype/nexus3:3.74.0
```
9. delete and install again
```
sudo docker stop nexus12
```
```
sudo docker rm nexus 12
```
```
 sudo docker volume rm nexus-data
 ```
  Repository
### nexus errors
When trying to pull Docker images from our Nexus repository, we encounter the following error:
```
http: server gave HTTP response to HTTPS client
```
This happens because our Nexus Docker repository is configured to use HTTP, but Docker by default expects all registries to use HTTPS for security reasons.

Solution
To allow Docker to pull images from an insecure (HTTP) registry, we need to configure the Docker daemon to trust the Nexus registry.


1. Find the IP Address or Domain Name of Your Nexus Server
Example: 192.168.1.100:8082 or nexus.example.com:8082

2. Edit Docker's daemon.json File
Open the file (create it if it doesn't exist):
```
sudo nano /etc/docker/daemon.json
```
3. Add the Insecure Registry Configuration
Add or modify the file to include the following:
```
{
  "insecure-registries": ["192.168.1.100:8082"]
}
```
Replace 192.168.1.100:8082 with your actual Nexus IP and port.


If there are already other settings inside daemon.json, make sure the JSON syntax remains valid (commas between entries, proper braces {}).

4. Restart Docker Service
After saving the file, restart Docker to apply the changes:
```
sudo systemctl restart docker
```
5. Test the Configuration
Try pulling an image from your Nexus repository:
```
docker pull 192.168.1.100:8082/your-repo/your-image:tag
```
If everything is set correctly, the pull should now succeed without HTTPS errors.



### Writing a Dockerfile

#### 1. Navigate to Your Project Directory
Let’s say your project is in a folder called my-python-app on your desktop.
In the terminal, type this command and press Enter:
```
cd Desktop/my-python-app
```

#### 2. Create the Dockerfile
In the terminal, type this command and press Enter:
```
vim dockerfile
```
#### 3. Write the First Line - Specify the Base Image
In the Dockerfile, type this as the first line:
```
FROM python:3.9
```

* FROM is a Docker instruction (a command) that tells Docker which base image to use.
* python:3.9 means we’re using an official Python image from Docker Hub, version 3.9.
* No spaces before or after FROM, just FROM python:3.9.

#### 4. Set the Working Directory
```
WORKDIR /app
```

* WORKDIR sets the folder inside the Docker container where all future commands will run.
* /app is the name of the folder (it’s created automatically inside the container).
* So, it’s like saying: “Hey Docker, do everything in the /app folder from now on.”

#### 5. Copy Your Project Files
```
COPY . .
```

* COPY tells Docker to copy files from your computer to the container.
* The first . (dot) means “everything in the current folder on my computer” (the my-python-app folder).
* The second . (dot) means “put it in the current folder in the container” (which is /app because of WORKDIR).
* There’s a space between the two dots: COPY . . (not COPY..).

#### 6. Install Dependencies
Let’s say your Python app has a requirements.txt file listing its dependencies (like Flask or other libraries).
```
RUN pip install -r requirements.txt
```

* RUN tells Docker to execute a command inside the container.
* pip install -r requirements.txt is the command to install Python packages listed in requirements.txt.
* There’s a space after RUN, then the command.
* better to use RUN apt update && apt install instead of run them sepretely Because every RUN creates a new layer in the image. The fewer layers, the smaller and more efficient your image will be. Combining commands into one RUN reduces the number of layers, making the image size smaller.

So, use && to chain related commands and avoid adding extra layers.

#### 7. Specify the Command to Run Your App
```
CMD ["python", "app.py"]
```
* CMD tells Docker what to run when the container starts.
* ["python", "app.py"] is a list (in square brackets []) that says: “Run the app.py file with Python.”
* Use quotes " around python and app.py, and separate them with a comma ,.
* No space between CMD and [, just CMD ["python", "app.py"].
* This assumes your Python app’s main file is called app.py.
8. Save the Dockerfile

Here’s what your complete Dockerfile looks like:
```
FROM python:3.9

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

CMD ["python", "app.py"]
```
#### 8. Build the Docker Image

A Docker "image" is like a blueprint or a packaged version of your app, including Python, your code, and dependencies.

We need to "build" this image from the Dockerfile.


Make sure you’re still inside the my-python-app folder (you can check with dir on Windows or ls on Linux/Mac to see if Dockerfile is there).
```
docker build -t my-python-app .
```

* docker: The Docker command-line tool.
* build: Tells Docker to build an image from a Dockerfile.
* -t: A flag (option) that means "tag" or "name the image."
* my-python-app: The name you’re giving the image (you can call it anything, like my-app or cool-project).
* . (dot): Means "use the Dockerfile in the current folder."

Docker reads the Dockerfile line by line:

* Downloads the python:3.9 image (if not already downloaded).
* Creates the /app folder.
* Copies your files into /app.
* Runs pip install -r requirements.txt to install dependencies.
* Prepares the CMD instruction for later.*
You’ll see output in the terminal showing each step. When it’s done, it says something like "Successfully built [image-id]".

#### 9. Run the Docker Container

A "container" is a running instance of your image—like starting the app based on the blueprint.
```
docker run -d -p 3000:3000 my-python-app
```

* run: Tells Docker to start a container from an image.
* my-python-app: The name of the image you built.
* -p: Port mapping.
* 3000:3000: Maps port 3000 on your computer to port 3000 in the container.
* -d : running in the background
Docker starts a container and executes the CMD ["python", "app.py"] from the Dockerfile.

If app your app.py` prints something (like "Hello, World!"), you’ll see it in the terminal.

If it’s a web app (e.g., Flask), it might say "Running on that URL in your browser.
Note:
If your app runs a web server (like Flask), add -p 3000:3000 to map ports:

Common CMD Commands in Dockerfile for Popular Languages:

| Language       | Typical Run File        | CMD in Dockerfile                     | Notes |
|----------------|--------------------------|----------------------------------------|-------|
| **Node.js**    | `index.js` / via `npm`   | `CMD ["npm", "start"]`                 | Requires `start` script in `package.json` |
|                | `index.js`               | `CMD ["node", "index.js"]`             | Direct run without `npm` |
| **Python**     | `app.py`                 | `CMD ["python3", "app.py"]`            | Use `python` if Python 3 is default |
| **Java**       | `app.jar`                | `CMD ["java", "-jar", "app.jar"]`      | JAR should be built before Docker build |
| **Go**         | compiled binary `app`    | `CMD ["./app"]`                        | Build binary before copy |
| **PHP**        | `index.php`              | `CMD ["php", "-S", "0.0.0.0:8000"]`    | Starts PHP built-in dev server |
| **Ruby**       | `app.rb`                 | `CMD ["ruby", "app.rb"]`               | |
| **.NET Core**  | `myapp.dll`              | `CMD ["dotnet", "myapp.dll"]`          | Requires SDK/runtime in image |
| **Rust**       | compiled binary `app`    | `CMD ["./app"]`                        | Use multi-stage builds for smaller images |
| **Shell Script** | `start.sh`             | `CMD ["sh", "start.sh"]`               | Or `bash` if needed |

#### Difference Between CMD and ENTRYPOINT in Dockerfile

In a Dockerfile, the CMD and ENTRYPOINT instructions define what command runs when a container starts from the image. While they may seem similar, they serve distinct purposes and can be used together for greater flexibility. Below, their differences are explained in a simple and clear manner.

##### CMD Instruction

The CMD instruction specifies the default command to run when a container starts. If no command is provided during container execution, CMD is used. However, if a command is specified, it overrides the CMD.

Example:

Suppose your Dockerfile contains:
```
CMD ["echo", "Hello World"]
```

Running docker run myimage outputs 
* "Hello World".

Running docker run myimage echo "Goodbye" outputs
* "Goodbye".

##### ENTRYPOINT Instruction

The ENTRYPOINT instruction defines a command that is always executed when the container starts. Unlike CMD, if arguments are provided during container execution, they are appended to the ENTRYPOINT command, not replaced.

Example:

Suppose your Dockerfile contains:
```
ENTRYPOINT ["echo"]
```
Running docker run myimage Hello outputs 
* "Hello".
Running docker run myimage without arguments 
* may produce no output (depending on the command).

##### Using ENTRYPOINT and CMD Together

When used together, ENTRYPOINT sets the main command, and CMD provides its default arguments. This allows a fixed command with customizable arguments.

Example:

Suppose your Dockerfile contains:
```
ENTRYPOINT ["echo"]
CMD ["Hello World"]
```

Running docker run myimage outputs 
* "Hello World".

Running docker run myimage Goodbye outputs 
* "Goodbye".

###### Key Differences

* **`CMD`**: Sets a default command that can be completely overridden.
* **`ENTRYPOINT`**: Sets a fixed command, with additional arguments appended.
  * **`Together`**: ENTRYPOINT keeps the command fixed, while CMD provides default arguments that can be changed.

###### When to Use Which?

* Use **`CMD`** for a default command that users can override.

* Use **`ENTRYPOINT`** for a command that must always run, with optional arguments.

* Use **`both`** for a fixed command with default, changeable arguments.

#### Dockerfile Best Practices

##### 1. Choose the Right Base Image

Selecting an appropriate base image is foundational to a good Dockerfile. Official Docker images, such as those found on Docker Hub, are curated, tested by millions, and regularly updated, making them a reliable starting point. Minimal images like Alpine (under 6 MB) reduce image size, improve portability, and minimize vulnerabilities compared to larger images like Ubuntu. However, consider potential trade-offs, such as Alpine’s performance limitations for certain applications, as noted in Python Speed. Always verify the source of custom images and prefer Verified Publisher or Docker-Sponsored Open Source images for trustworthiness.
```
FROM alpine:3.21@sha256:a8560b36e8b8210634f77d9f7f9efd7ffa463e380b75e2e74aff4511df3ef88c
```
##### 2. Use Multi-Stage Builds

Multi-stage builds allow you to separate the build environment (e.g., compiling code) from the runtime environment, resulting in smaller, more secure images. By using multiple FROM statements, you can copy only the necessary artifacts to the final image, excluding build tools or temporary files. This approach is particularly effective for languages like Go or Java, where build dependencies are significant.

```
FROM golang:1.20 AS builder
WORKDIR /app
COPY . .
RUN go build -o myapp

FROM alpine:latest
COPY --from=builder /app/myapp /usr/local/bin/myapp
CMD ["myapp"]

```
##### 3. Leverage .dockerignore Files

A .dockerignore file excludes unnecessary files from the build context, reducing build time and image size. Common exclusions include documentation files (e.g., *.md), logs, or dependency directories like node_modules. This practice also prevents sensitive files from being accidentally included.
```
# .dockerignore
*.md
*.log
node_modules
```
##### 4. Minimize Image Layers

Docker creates a new layer for each instruction, with a maximum of 127 layers. To reduce layers, combine commands in a single RUN instruction using && and clean up temporary files in the same command to avoid bloating the image.
```
RUN apt-get update && apt-get install -y curl git && rm -rf /var/lib/apt/lists/*
```
##### 5. Run as Non-Root User

Running containers as root increases the risk of privilege escalation. Create a non-root user with a high UID/GID (e.g., above 10,000) to enhance security. Ensure the user has appropriate permissions for application files and avoid using privileged ports like 80.
```
RUN groupadd -g 10001 appuser && useradd -u 10000 -g appuser appuser
USER appuser:appuser
```

##### 6. Avoid Unnecessary Packages

Installing only essential packages reduces image size, build time, and vulnerabilities. Use flags like --no-install-recommends for apt-get to avoid unnecessary dependencies.
```
RUN apt-get update && apt-get install -y --no-install-recommends curl && rm -rf /var/lib/apt/lists/*
```
##### 7. Prefer COPY Over ADD

COPY is more predictable and secure than ADD, which can extract tar files or download from URLs, potentially introducing vulnerabilities. Use ADD only for specific cases like tar auto-extraction.
```
COPY . /app
```

##### 8. Leverage Build Cache

Docker’s build cache reuses layers from previous builds, speeding up the process. Place commands that change infrequently (e.g., installing dependencies) before those that change often (e.g., copying application code) to maximize cache reuse.
```
FROM python:3.9
RUN pip install -r requirements.txt
COPY . /app
```

##### 9. Pin Base Image Versions

Pinning base images to specific versions or digests ensures reproducibility and prevents unexpected changes. Tools like Docker Scout can help monitor updates.
```
FROM python:3.9.1@sha256:example-digest
```

##### 10.  Use Environment Variables

Environment variables allow flexible configuration without modifying the Dockerfile. Avoid hardcoding sensitive data like API keys; instead, pass them at runtime or use build arguments.
```
ENV APP_PORT=8080
```

##### 11.  Combine ENTRYPOINT and CMD

Use ENTRYPOINT for the main executable and CMD for default arguments, allowing users to override arguments easily.
```
ENTRYPOINT ["python", "app.py"]
CMD ["--port", "8080"]
```

##### 12.  Keep Images Minimal and Focused

Each container should serve a single purpose, following the “one concern per container” principle. This improves scalability and reduces complexity. Use minimal images like gcr.io/distroless for languages like Python or Go to further reduce size and attack surface.

##### 13.  Use Build Arguments

Build arguments (ARG) allow dynamic configuration during the build process, making Dockerfiles more flexible.
```
ARG PYTHON_VERSION=3.9
FROM python:${PYTHON_VERSION}
```

##### 14.  Expose Only Necessary Ports

Expose only the ports required by your application to minimize the attack surface. The EXPOSE instruction is informational and should align with runtime publishing.
```
EXPOSE 8080
```

##### 15.  Lint and Scan Dockerfiles

Use tools like hadolint to lint Dockerfiles for common issues, such as running as root or using ADD inappropriately. Scan images in CI/CD pipelines with tools like Sysdig’s inline scanner to detect vulnerabilities early.

##### 16.  Regularly Update Base Images

Outdated base images can introduce vulnerabilities. Rebuild images periodically and use stable or LTS versions to balance updates with stability.

##### 17.  Avoid Hardcoding Sensitive Data

Never include secrets like API keys in Dockerfiles. Use environment variables, secrets management tools, or runtime injection to handle sensitive data securely.

##### 18.  Order Commands for Cache Optimization

Place RUN commands for installing dependencies before COPY or ADD commands to leverage caching, as application code is likely to change more frequently.

##### 19.  Use Distroless or Scratch Images

Distroless images (e.g., gcr.io/distroless/python) or FROM scratch provide minimal environments with fewer dependencies, reducing vulnerabilities and image size.

##### 20.  Secure the Docker Socket

Avoid mounting /var/run/docker.sock unless necessary, as it can allow unauthorized access to the host. Secure TCP connections if Docker is exposed remotely.

### Docker network 
Docker networking is how containers—those isolated environments running your applications—talk to each other and the outside world. Think of containers as little boxes: networking is the phone line connecting them or letting them call out.

* Isolation: Each container has its own network space, keeping things secure and separate.
* Communication: Containers often need to work together, like a web app chatting with a database.
* External Access: Sometimes, you want the world (or at least your computer) to reach a container, like accessing a website.

#### The Basics of Docker Networking
Docker uses network drivers to control how containers connect. These are like different types of roads containers can travel on. Here are the main ones:

1. **`Bridge`**: The default "road" for containers on the same computer.
2. **`Host`**: Containers share the computer's network directly.
3. **`Overlay`**: Connects containers across multiple computers.
4. **`Macvlan`**: Gives containers their own network identity, like real devices.
5. **`None`**: No networking at all—total isolation.

When you install Docker, it sets up three default networks:

* bridge: For basic container connections.
* host: For using the computer’s network.
* none: For no networking.

##### 1. **`Bridge Network: The Starting Point`**
The bridge network is Docker’s default. It’s like a virtual switch connecting containers on the same computer. If you don’t pick a network, your container uses this one.

**`Example 1: Running a Container on the Default Bridge`**
```
docker run -d --name web1 nginx
```
* **`-d`**: Runs it in the background.
* **`--name web1`**: Names the container "web1".
* **`nginx`**: The image (a web server).


Check the network details:
```
docker network inspect bridge
```
* This shows the bridge network’s info, including web1’s IP address (e.g., 172.17.0.2). Each container gets its own IP here.
* Containers on the bridge network can ping each other using IPs. But Docker also has a built-in DNS system for names.

**`Example 2: Connecting Two Containers`**
```
docker run -d --name web2 nginx
```
Now, from web1, ping web2 by name:
```
docker exec -it web1 ping web2
```
* **`docker exec -it`**: Runs a command inside web1.
* **`ping web2`**: Tests if web1 can reach web2.

It works! Docker’s DNS translates web2 to its IP automatically.


**`Example 3: Mapping a Port`**
To access a container from your computer, map a container port to a host port.
```
docker run -d -p 8080:80 --name web3 nginx
```
* **`-p 8080:80`**: Maps port 8080 (host) to port 80 (container).
Open your browser and go to http://localhost:8080. You’ll see nginx’s welcome page!

##### 2. **`Custom Bridge Networks: Leveling Up`**
The default bridge has limits (e.g., DNS isn’t always automatic). Create your own bridge network for more control.

**`Example 4: Making a Custom Network`**

Create a network called mynetwork:
```
docker network create mynetwork
```
Run two containers on it:
```
docker run -d --network mynetwork --name web4 nginx
```
```
docker run -d --network mynetwork --name web5 nginx
```
Test communication:
```
docker exec -it web4 ping web5
```
It works, and DNS resolves names perfectly. Custom networks are better for projects with multiple containers.

##### **`3. Host Network: No Isolation`**
The host network lets a container use the computer’s network directly. No separate IP—it’s like the container is part of the host.

**`Example 5: Using the Host Network`**
```
docker run -d --network host --name web6 nginx
```
Since it’s on the host network, nginx’s port 80 is directly on your computer. Visit http://localhost:80—it works! No port mapping needed (or allowed).

* Note: This reduces isolation, so use it carefully.

##### 4. **`None Network: Total Isolation`**
The none network cuts off all networking. It’s perfect for containers that don’t need to talk to anyone. 
* You cannot directly transition a Docker container from none network mode to host network mode 
**`Example 6: No Networking`**
```
docker run -d --network none --name isolated busybox sleep 3600
```
* **`busybox`**: A tiny image.
* **`sleep 3600`**: Keeps it running for an hour.
Try pinging from it:
```
docker exec -it isolated ping google.com
```
It fails—no network, no connection!

##### 5. **`Overlay Network: Multi-Computer Magic`**
The overlay network connects containers across different computers. It’s used with Docker Swarm (a tool for managing multiple Docker hosts).

**`Example 7: Setting Up an Overlay Network`**
```
docker swarm init
```
Create an overlay network:
```
docker network create -d overlay myoverlay
```
Run a service:
```
docker service create --network myoverlay --name web8 nginx
```
Now, containers (or services) on myoverlay can talk across hosts. This is advanced, so try it once you’re comfortable with single-host setups.

##### 6. **`Macvlan Network: Containers as Devices`**
The macvlan network gives containers their own MAC address and IP, making them act like physical devices on your network.

**`Example 8: Creating a Macvlan Network`**
Set up a network (adjust eth0 to your network interface):
```
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  mymacvlan

```
Run a container with a specific IP:
```
docker run -d --network mymacvlan --ip=192.168.1.100 --name web7 nginx
```
* Access it at http://192.168.1.100 from another device on your network!

* Caution: Ensure IPs don’t conflict with other devices.

#### Network Scopes
**`Local`**: For one computer (bridge, host, etc.).
**`Swarm`**: Across multiple computers (overlay).
#### Service Discovery
In Swarm, containers find each other by service names—super handy for big setups.

#### Load Balancing
Swarm balances traffic across multiple containers automatically.

#### Network Policies
Control who talks to whom (more common in tools like Kubernetes).

#### Docker Network Connect

The docker network connect command attaches a running or stopped container to an existing network, allowing it to communicate with other containers or services on that network.

Syntax:
```
docker network connect [OPTIONS] NETWORK CONTAINER
```
* **`Example: Connect a container to a network named "my-network`**
```
docker network connect my-network my-container
```
This connects my-container to my-network, enabling communication with other containers in that network.

#### Docker Network Disconnect

The docker network disconnect command detaches a container from a specified network, isolating it from other containers or services on that network.

Syntax:
```
docker network disconnect [OPTIONS] NETWORK CONTAINER
```
* **`Example: Disconnect a container from "my-network`**
```
docker network disconnect my-network my-container
```
This removes my-container from my-network, stopping communication with that network's resources.


* A container can be connected to multiple networks simultaneously.
* Use docker network ls to list available networks.
* Use docker inspect <container> to verify a container's network connections.
* The container's primary network (set at creation with --network) cannot be disconnected without stopping and recreating the container.
* Always ensure the container and network names or IDs are correct to avoid errors.

### Docker socket

The Docker socket is a Unix socket typically located at /var/run/docker.sock. It acts as a bridge, allowing the Docker CLI to send commands to the Docker daemon. The daemon is responsible for managing containers, images, networks, and more. Every time you run a Docker command like docker run or docker ps, the CLI communicates with the daemon through this socket.

#### Connecting the Docker Socket to a Container
One common use of the Docker socket is to mount it into a container. This allows the container to send commands to the host's Docker daemon, effectively managing other containers. This is particularly useful for tools like CI/CD systems (such as Jenkins) that need to start, stop, or manage containers as part of their workflow.

* **`Example: Running Jenkins with Access to the Docker Socket`**
Suppose you want to run Jenkins in a container and allow it to manage other containers on the host. You can achieve this by mounting the host's Docker socket into the Jenkins container:
```
docker run -v /var/run/docker.sock:/var/run/docker.sock -d jenkins/jenkins
```

* **`-v /var/run/docker.sock:/var/run/docker.sock`** mounts the host's Docker socket to the same path inside the container.
* **`-d`** runs the container in detached mode.
* **`jenkins/jenkins`** is the image used to run Jenkins.
Once the container is running, Jenkins can use the Docker CLI inside the container to communicate with the host's Docker daemon and manage other containers.

#### Security Notes
Mounting the Docker socket into a container grants it full control over the host's Docker daemon. This means the container can perform any action the daemon can, such as starting or stopping containers, pulling images, and more. Therefore, this should only be done with trusted containers, as a compromised container could pose serious security risks.

* The Docker socket is essential for communication between the Docker CLI and the daemon.
* It is a Unix socket located at /var/run/docker.sock.
* Mounting the socket into a container allows it to control the host's Docker daemon.
* Always exercise caution when mounting the Docker socket to prevent security issues.

### Docker copy
The docker cp command allows you to transfer files or directories between a running (or stopped) Docker container and the host filesystem. It works similarly to the cp command in Linux but is designed for Docker environments.

Syntax:
```
docker cp [SOURCE_PATH] [DESTINATION_PATH]
```
* **`SOURCE_PATH`**: The file or directory to copy (on the host or in the container).

* **`DESTINATION_PATH`**: The destination for the copied file or directory (on the host or in the container).

* For containers, the path is specified as container_name:/path or container_id:/path.

#### Copying a File from Host to Container

To copy a file from the host system to a container, specify the host file path as the source and the container's path as the destination.

Example:

Suppose you have a file config.txt on your host at /home/user/config.txt and want to copy it to a container named my-container at /app/config.txt.
```
docker cp /home/user/config.txt my-container:/app/config.txt
```
This copies config.txt from the host to the /app directory inside my-container.

#### Copying a File from Container to Host

To copy a file from a container to the host, specify the container's file path as the source and the host path as the destination.

Example:

Suppose a container named my-container has a log file at /app/log.txt, and you want to copy it to /home/user/log.txt on the host.
```
docker cp my-container:/app/log.txt /home/user/log.txt
```
This copies log.txt from the container to the host's /home/user directory.

* **`Container State`**: The container can be running or stopped when using docker cp.

* **`Paths`**: Ensure the source file exists and the destination path is valid. For containers, use the format container_name:/path.

* **`Permissions`**: The copied files retain their permissions, but the user running docker cp must have appropriate access.

* **`Directories`**: To copy entire directories, the syntax is the same (e.g., docker cp my-container:/app /home/user/app).

* **`Verification`**: Use docker exec to check files inside the container (e.g., docker exec my-container ls /app).

* Always verify the container name or ID with docker ps or docker ps -a to avoid errors.

### Docker Volumes

Docker volumes are storage mechanisms that exist outside of a container's filesystem. They allow data to persist even if a container is stopped or removed and can be shared between containers. Volumes are managed by Docker or defined by the user, depending on the type.

#### Types of Docker Volumes

There are three main types of volumes in Docker:

##### 1. Anonymous Volumes:

Temporary volumes created automatically by Docker when no specific name is provided.

* Identified by a random ID and not easily reusable.

* Best for temporary or single-use data that doesn’t need to be referenced later.

##### 2. Named Volumes:

Volumes with a user-defined name, managed by Docker.

* Stored in Docker’s storage area (e.g., /var/lib/docker/volumes/ on Linux).

* Ideal for persistent data that needs to be reused or shared across containers.

##### 3. Bind Mounts:

Direct mappings of a specific folder or file from the host filesystem into the container.

* Not managed by Docker; the user controls the source path on the host.

* Useful when you need precise control over the data location or want to use existing host files.

Creating a Named Volume
```
docker volume create my-volume
```
Listing Volumes
```
docker volume ls
```
Inspecting a Volume
```
docker volume inspect my-volume
```
Removing a Volume
```
docker volume rm my-volume
```

**`Example 1: Using an Anonymous Volume for Temporary Data`**

**`Scenario`**: You’re running a container to process some temporary data, and you don’t need the data after the container stops.
```
docker run -d -v /app/data my-image
```

* **`The -v /app/data`** creates an anonymous volume mapped to /app/data inside the container.

* Docker assigns a random ID to this volume (visible via docker volume ls).

* The container writes data to /app/data, but when the container is removed, the volume is typically deleted unless explicitly preserved.

* Use Case: Temporary storage for one-off tasks, like processing a dataset that doesn’t need to be saved.

**`Example 2: Using a Named Volume for a Database`**

**`Scenario`**: You want to run a PostgreSQL database and ensure its data persists even if the container is deleted.
```
docker run -d -v postgres-data:/var/lib/postgresql/data postgres:latest
```
**`The -v postgres-data:/var/lib/postgresql/data`** uses a named volume called postgres-data.

* If postgres-data doesn’t exist, Docker creates it automatically.

* PostgreSQL stores its database files in /var/lib/postgresql/data, which is linked to the postgres-data volume.

* If the container is stopped or removed, the database data remains in postgres-data and can be reused by a new container.

* Use Case: Persistent storage for databases or applications where data must survive container restarts.

**`Example 3: Using a Bind Mount for Development`**

**`Scenario`**: You’re developing a web app and want the container to use code from a folder on your host machine, so changes are reflected instantly.
```
docker run -d -v /home/user/code:/app my-web-app
```

* **`The -v /home/user/code:/app`** creates a bind mount, linking the host’s /home/user/code folder to /app in the container.

* Any changes to files in /home/user/code on the host (e.g., editing code) are immediately visible in the container’s /app folder, and vice versa.

* Unlike Docker-managed volumes, the /home/user/code folder is entirely controlled by the host’s filesystem.

* Use Case: Real-time development, where you edit code on the host and want the container to use the updated files without rebuilding the image.

**`Example 4: Sharing a Named Volume Between Containers`**

**`Scenario`**: Two containers (e.g., a web app and a log processor) need to share the same log files.
```
docker volume create log-volume
docker run -d -v log-volume:/app/logs web-app
docker run -d -v log-volume:/app/logs log-processor
```

* The log-volume is a named volume shared by both containers.

* The web-app container writes logs to /app/logs, which are stored in log-volume.

* The log-processor container reads from the same /app/logs path, accessing the same log files.

* This setup ensures both containers work with the same data seamlessly.

* Use Case: Sharing data like logs, configuration files, or datasets between multiple containers.

| Type        | Managed by Docker? | Persistent? | Reusable? | Example Use Case                      |
|-------------|--------------------|-------------|-----------|----------------------------------------|
| Anonymous   | Yes                | Temporary   | No        | Temporary data for one-off tasks       |
| Named       | Yes                | Yes         | Yes       | Persistent database storage            |
| Bind Mount  | No                 | Yes         | Yes       | Development with host code             |

* Use anonymous volumes for temporary data you don’t need to keep.

* Use named volumes for important data that needs to persist or be shared.

* Use bind mounts when you need direct control over files on the host.

* Check volumes with docker volume ls to avoid clutter.

* Be cautious with bind mounts, as incorrect host paths can cause errors.

### Docker Compose
Docker Compose is a tool for defining and running multi-container Docker applications. It uses a YAML file (usually called docker-compose.yml) to describe your application's components—called services—like a web server or database, how they connect, and where their data is stored. With one command, you can start or stop everything, making it perfect for development, testing, and even small-scale production setups.

#### Why Use Docker Compose?
* **`Easier Management`**: Define everything in one file instead of running multiple Docker commands.
* **`Consistency`**: Ensures your application runs the same way on every machine.
* **`Quick Setup`**: Spin up complex environments fast, great for developers and students.
* **`Teamwork`**: Share the file with others to replicate your setup exactly.
* **`Flexibility`**: Adjust and scale your app with minimal effort.

#### Key Concepts of Docker Compose
Before we jump into examples, let’s cover the essentials:

* **`Services`**: These are the containers in your app, like a web server or a database. Each service is defined in the Compose file and runs its own container.
* **`Networks`**: These let your services talk to each other. Docker Compose automatically creates a network, but you can customize it.
* **`Volumes`**: These store data so it’s not lost when containers stop. Think of them as external hard drives for your containers.

#### The Docker Compose File
The docker-compose.yml file is where everything happens. It’s written in YAML, a simple format that’s easy to read and write. Here’s what it typically includes:

**`Version`**: The format of the file (e.g., '3.8').
**`Services`**: The containers you want to run.
**`Networks`**: How services connect.
**`Volumes`**: Where data is saved.
Here’s a basic example structure:
```
version: '3.8'
services:
  app:
    image: my-app-image
    ports:
      - "8000:80"
    volumes:
      - app_data:/app
    networks:
      - my_network
volumes:
  app_data:
networks:
  my_network:

```

**`Let’s create a docker-compose.yml file for a basic web application with a web server and a database. Here’s the file:`**
```
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    volumes:
      - web_data:/usr/share/nginx/html
    networks:
      - app_network
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
    volumes:
      - db_data:/var/lib/mysql
    networks:
      - app_network
volumes:
  web_data:
  db_data:
networks:
  app_network:

```

* **`version: '3.8'`**: This tells Docker Compose which version of its file format we’re using. Version 3.8 is modern and supports most features. It’s like saying, “This is the rulebook I’m following.”

* **`services`**: This section lists all the containers (services) in your app. Here, we have two: web and db.

* **`web`**: (The Web Server):

* **`image`**: nginx:latest:
This uses the official Nginx image from Docker Hub (a library of pre-built images). Nginx is a popular web server, and latest grabs the newest version.

* **`ports: - "8080:80"`**:
This maps port 8080 on your computer (the host) to port 80 inside the container, where Nginx listens. So, you can visit http://localhost:8080 in your browser to see the web server.

* **`volumes: - web_data:/usr/share/nginx/html:`**
This links a volume called web_data to the folder /usr/share/nginx/html inside the container. That’s where Nginx looks for web files (like HTML pages). If you put files in web_data, they’ll show up on the site.

* **`networks: - app_network`**:
This connects the web service to a network called app_network, so it can talk to other services (like db).

* **`db: (The Database)`**:
* **`image: mysql:5.7`**:
This uses MySQL version 5.7, a widely-used database. The image comes from Docker Hub.

* **`environment: MYSQL_ROOT_PASSWORD`**:
This sets an environment variable inside the container. MySQL uses it to set the root user’s password to “example.” Environment variables configure the service without changing the image.

* **`volumes: - db_data:/var/lib/mysql`**:
This links a volume called db_data to /var/lib/mysql, where MySQL stores its database files. Even if the container stops, your data stays safe in db_data.

* **`networks: - app_network`**:
This puts the db service on the same network as web, so they can communicate (e.g., the web app could query the database).

* **`web_data: and db_data`**:
These declare two named volumes. They’re like storage buckets managed by Docker. Without them, data would vanish when containers stop.

* **`app_network`**:
This creates a custom network for our services. They can find each other by their service names (web or db) instead of IP addresses.

##### What Happens When You Run This?

* Type docker-compose up in your terminal (in the same folder as the file).

* Docker Compose downloads the Nginx and MySQL images, starts the containers, sets up the network, and creates the volumes.

* Visit http://localhost:8080 to see Nginx’s default page. The MySQL database runs in the background with the password “example.”

#### Restart Policies in Docker Compose
When running services using Docker, container stability is a primary concern. Containers can stop for various reasons, such as an application error, a memory issue, or a full system reboot. A restart policy instructs the Docker daemon on how to behave when a container exits.

This policy is defined directly within the docker-compose.yml file for each service, helping to ensure the reliability and high availability of your applications.

Docker Compose supports four primary restart policies:

1. **`no`**

2. **`always`**

3. **`on-failure`**

4. **`unless-stopped`**

##### 1.**`no (Default)`**
This is the default policy. If a container stops, the Docker daemon will never attempt to restart it automatically.

Usage:
```
services:
  my-task:
    image: my-image:latest
    restart: "no"
```

Note: Since this is the default behavior, omitting the restart key for a service has the same effect.

When to Use:

**`One-off Tasks`**: Ideal for containers that perform a single, finite task and are expected to exit upon completion. Examples include running a database migration script, a data processing job, or a backup task.

**`Development & Debugging`**: When you want full manual control over a container to inspect its logs and understand why it stopped.

##### 2. **`always`**
This policy instructs the Docker daemon to always restart the container if it stops, regardless of the exit code.

Usage:
```
services:
  web-server:
    image: nginx:latest
    restart: always
```

When to Use:

Critical Services: Best for services that must be running at all times, such as web servers, API gateways, or message queues.

Important Note: This policy will even restart a container that you manually stopped with docker stop if the Docker daemon itself is restarted (e.g., after a server reboot). Its goal is maximum availability, sometimes at the cost of manual control.

##### 3. **`on-failure`**
This policy restarts a container only if it exits with a non-zero exit code, which signifies an error. If the container stops gracefully with an exit code of 0 (indicating success), it will not be restarted.

You can also specify a maximum number of restart attempts.

Usage:
```
services:
  worker:
    image: my-worker-image
    restart: on-failure:5 # Attempts to restart a maximum of 5 times
```

If you omit the retry count (e.g., restart: on-failure), the daemon will try to restart it indefinitely.

When to Use:

Worker Services: Excellent for background jobs or workers that might encounter transient errors (like a temporary loss of database connectivity). This gives them a chance to recover and restart.

Computational Tasks: If a task might fail due to a recoverable error, this policy allows it to be retried automatically.

##### 4. **`unless-stopped`**
This policy is very similar to always. It will always restart a container if it stops, with one critical exception: it will not restart a container that was explicitly stopped by a user (via the docker stop command).

Usage:
```
services:
  database:
    image: postgres:14
    restart: unless-stopped
```

When to Use:

Most Production Scenarios: This is generally the recommended policy for long-running services like databases, backends, and other core application components.

It ensures that services come back online after a system reboot or an unexpected crash, but it also respects deliberate administrative actions. If you stop a container for maintenance, you can be confident it will stay down until you manually start it again.

#### Key Difference: always vs. unless-stopped

Understanding the distinction between these two is crucial for managing production environments effectively. The difference lies in how they handle a manually stopped container across a Docker daemon restart.

Let's consider a scenario: You need to perform maintenance and manually stop a container using docker stop my-container.

**`With restart: always:`**
The container stops as requested. However, if the entire server reboots (and the Docker daemon restarts), the daemon will see that the container is supposed to be "always" running and will automatically start it again. This can be undesirable if your maintenance is not yet complete.

**`With restart: unless-stopped:`**
The container stops as requested. Now, if the server reboots, the Docker daemon remembers that the container's state was intentionally set to "stopped" by the user. As a result, it will not automatically start the container. It will remain stopped until you explicitly start it with docker start my-container

#### Advanced Features of Docker Compose

##### Environment Variables
You can customize services using environment variables. Add more to the db service:
```
services:
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: myapp_db
      MYSQL_USER: user
      MYSQL_PASSWORD: pass

```

* MYSQL_DATABASE: Creates a database called myapp_db.

* MYSQL_USER and MYSQL_PASSWORD: Add a user “user” with password “pass” for safer access.

##### Dependencies Between Services
If your web app needs the database to start first, use depends_on:
```
services:
  web:
    image: nginx:latest
    ports:
      - "8080:80"
    depends_on:
      - db
  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example

```     
* depends_on: - db: Ensures db starts before web. Note: It doesn’t wait for the database to be fully ready—just started. For that, you might need extra scripting.

##### Scaling Services
Want multiple web servers? Scale them:
```
docker-compose up --scale web=3
```

* This runs three web containers. Each gets a unique name (e.g., web_1, web_2, web_3). Useful for load testing or distributed apps.

#### Docker Compose Commands

Start Everything:
```
docker-compose up
```

* Add -d to run in the background: docker-compose up -d.

Stop Everything:
```
docker-compose down
```

* Stops and removes containers and networks. Add --volumes to delete volumes too.

Build Custom Images:
```
docker-compose build
```

* If a service uses a build section (e.g., from a Dockerfile), this builds it.

Check Logs:
```
docker-compose logs
```

* See output from all services. Add -f to watch live: docker-compose logs -f.
Run a Command:
```
docker-compose run db mysql -u root -p
```

* Runs the MySQL client in the db service to interact with the database.

Docker Compose takes the complexity out of managing multi-container apps. With a single file, you can define services, connect them, and persist data—all launched with one command. This guide covered the basics (services, networks, volumes), walked through a detailed example, and introduced advanced features like scaling and dependencies. Now, you’re ready to build your own setups, troubleshoot issues, and even teach Docker Compose to others. Experiment with it, tweak the examples, and watch your Docker skills soar!

### Docker Swarm
Docker Swarm is a tool that helps you manage multiple Docker containers across several machines. Imagine it as a team leader organizing a group of workers (containers) to complete tasks efficiently. It’s a built-in solution from Docker to orchestrate and scale your applications.

* **`Node`**: A computer (physical or virtual) in the Swarm.

* **`Service`**: Instructions for what a container should do, like running a website.

* **`Task`**: A single running container based on a service.

* **`Stack`**: A group of services that form an application.

#### Swarm has two types of nodes:

* **`Manager Nodes`**: These lead the Swarm, assign tasks, and keep everything running smoothly.

* **`Worker Nodes`**: These follow the manager’s instructions and run the tasks.

#### Setting Up a Swarm
To create a Swarm, you start with one machine as the manager. Use this command:
```
docker swarm init
```
This makes your machine the manager node. 

To add more machines (worker nodes), the manager gives you a special code (token). On another machine, run:
```
docker swarm join --token <token> <manager-ip>:<port>
```
Now your Swarm is growing!

#### Deploying Services in docker swarm
With your Swarm ready, you can deploy services. For example, to run a web server with three copies:
```
docker service create --name webserver --replicas 3 nginx
```
This creates a service called "webserver" with three instances of the Nginx container, spread across your nodes. 
To increase or decrease the number of copies:
```
docker service scale webserver=5
```
This changes it to five instances.

#### Managing the Swarm
You need to keep an eye on your Swarm. Check all services with:
```
docker service ls
```
To see details about a specific service:
```
docker service ps webserver
```
If you update a service (like using a newer version of Nginx):
```
docker service update --image nginx:1.19 webserver
```
If something goes wrong, Swarm lets you undo the change.

#### Benefits of Using Docker Swarm
* **`Simple`**: It uses the same Docker commands you already know.

* **~`Scalable`~**: Add or remove copies of your service as needed.

* **`Works with Docker`**: Fits perfectly with other Docker tools.

## Web server & Application server

### What is a Web Server?
A web server is a piece of software (and sometimes the hardware it runs on) that handles requests from your web browser and sends back the content you see on a webpage. Think of it as a delivery service: when you type a URL (like www.example.com) into your browser, the web server grabs the files—such as HTML for the page structure, CSS for styling, JavaScript for interactivity, and images or videos—and delivers them to you. This content is usually static, meaning it doesn’t change unless someone updates the files.

Web servers use the HTTP or HTTPS protocol to communicate with browsers. They can also handle some basic dynamic content (content that changes, like a weather update) using simple tools like CGI scripts or server-side includes, but that’s not their main strength. Examples of popular web servers include:

* Apache HTTP Server: A widely-used, open-source option.
* Nginx: Known for being fast and efficient.
* Microsoft IIS: A web server from Microsoft, often used with Windows systems.

### What is an Application Server?
An application server is a more powerful software framework that goes beyond just serving webpages. It’s like a backstage manager for complex applications, providing an environment where programs can run, process data, and handle the heavy lifting of an app’s logic. For example, in an online store, the application server might manage your shopping cart, process your payment, and update the inventory—all while talking to a database.

Application servers are built to support dynamic content (content generated on the fly) and business logic (the rules and processes that make an app work). They often include advanced features like transaction management, security, and connections to other systems. Common examples include:

* Apache Tomcat: A popular choice for Java-based web applications.
* JBoss/WildFly: Open-source servers with enterprise features.
* GlassFish, Oracle WebLogic, IBM WebSphere: Heavy-duty options for big business applications.

| Feature                | Web Server                                        | Application Server                                         |
|------------------------|---------------------------------------------------|------------------------------------------------------------|
| **Primary Job**        | Serve static content and handle HTTP requests     | Run applications and manage business logic                 |
| **Content Type**       | Static files (HTML, CSS, JS, images)              | Dynamic content (generated by app logic)                   |
| **Dynamic Content**    | Limited (e.g., CGI, server-side includes)         | Extensive (e.g., servlets, EJBs, ASP.NET)                  |
| **Business Logic**     | Not usually handled                               | Handles complex rules and processes                        |
| **Database Interaction**| Indirect (via scripts or add-ons)                | Direct (with tools like connection pooling)                |
| **Transaction Management** | Not supported                             | Supported (even across multiple systems)                   |
| **Scalability**        | Basic load balancing                              | Advanced clustering and load balancing                     |
| **Security**           | Basic (e.g., SSL, simple authentication)          | Advanced (e.g., user roles, encryption)                    |
| **Protocols**          | Mostly HTTP/HTTPS                                 | HTTP/HTTPS plus others (e.g., RMI, JMS)                    |
| **Examples**           | Apache HTTP Server, Nginx, IIS                    | Tomcat, JBoss, GlassFish, WebLogic, WebSphere     

Breaking Down the Differences


* **Focus**: Web servers are all about efficiently delivering files over the internet. Application servers focus on running the programs that create dynamic content and manage app operations.
* **Complexity**: Web servers are simpler, built for speed and serving content. Application servers are more complex, offering a full toolkit for developers to build and run sophisticated apps.
* **Flexibility**: A web server might struggle with heavy app logic, while an application server can handle both static and dynamic tasks (though it’s overkill for just static files).

#### How They Work Together
In many setups, web servers and application servers team up to deliver a seamless experience. Here’s how they often fit into the picture:

##### 1. Separate Roles
Flow: Your browser sends a request to the web server, which serves static files (like images) directly. If the request needs dynamic content (like a personalized dashboard), the web server passes it to the application server. The application server processes the request, talks to a database if needed, and sends the result back through the web server to your browser.
Why?: This split makes things faster (web servers are great at static content) and more secure (the application server stays protected behind the web server).
Example: Nginx (web server) forwards requests to Tomcat (application server) for a Java app.
```
Browser --> Web Server --> Application Server --> Database
```
##### 2. All-in-One Application Server
Flow: Some application servers can handle HTTP requests directly, serving both static and dynamic content without a separate web server.
Why?: Simpler setup, often used in smaller projects or when the app server has built-in web capabilities.
Example: A Java app running on JBoss, handling everything from HTTP requests to database queries.
```
Browser --> Application Server --> Database
```
##### Use a Web Server When:
* You’re running a static website, like a blog with fixed pages or a portfolio site.
* You need a simple web app with minimal dynamic features (e.g., a form that sends an email).
* You want a reverse proxy to boost performance or security in front of an application server.
##### Use an Application Server When:
* You’re building a complex web app, like an e-commerce site or a customer management system.
* Your app needs enterprise features, such as transaction tracking across systems or messaging between components.
* You require a scalable, secure setup for mission-critical applications.
##### Using Both Together
For many modern apps, combining them works best. For instance, Nginx might serve static files and cache responses, while passing dynamic requests to a Python app running on Gunicorn. This balances speed, efficiency, and power.

#### Why This Matters Today
In the past, the line between web servers and application servers was sharper. Now, it’s a bit blurry—some web servers (like Apache with PHP) can handle more dynamic tasks, and many application servers (like Tomcat) can serve web content directly. Plus, cloud platforms like AWS or Heroku often bundle these roles into one easy package. Still, knowing the difference helps you pick the right tools for your project and understand how systems are built.

### What is Nginx?
Nginx is a popular web server software. A web server is like a waiter in a restaurant: it takes requests from customers (users’ browsers) and delivers the requested "dishes" (web pages, images, or other files). But Nginx can do more than just serve web pages—it can also act as a reverse proxy (directing traffic to other servers) and a load balancer (distributing traffic across multiple servers to handle more users).

#### hat is the Nginx Configuration File?
The Nginx configuration file is a text file that tells Nginx how to behave. It’s like a set of instructions or a recipe that Nginx follows to know:

* Which websites to serve.
* Where the website files are located.
* How to handle different types of requests (e.g., for images, videos, or web pages).
* How to manage security settings (like enabling HTTPS).
* How to distribute traffic if you’re using load balancing.
* Without this file, Nginx wouldn’t know what to do—it’s essential for setting up and customizing your web server.

#### Where is the Configuration File Located?
For a standard Nginx installation (e.g., on a Linux server):
* The main configuration file is usually located at /etc/nginx/nginx.conf.
* Additional configuration files can be placed in /etc/nginx/conf.d/ (e.g., /etc/nginx/conf.d/mysite.conf).
For Docker users:
* You can create the configuration file in any directory on your computer and later mount it into the Docker container.
#### How to Write an Nginx Configuration File
Writing an Nginx configuration file involves using directives and blocks:

* **`Directives`**: These are single-line commands that end with a semicolon (;). For example, listen 80; tells Nginx to listen for requests on port 80.
* **`Blocks`**: These are sections that group related directives together. They start with a keyword and an opening curly brace ({), and end with a closing curly brace (}). For example, the server block defines settings for a specific website.

##### A simple Nginx configuration file typically includes:

* Events block: Controls how Nginx handles connections.
* HTTP block: Contains settings for handling web (HTTP) traffic.
* Inside the HTTP block, you can have one or more server blocks, each defining a specific website or service.
* Inside each server block, you can have location blocks to handle different parts of the website (e.g., different URLs).
##### Example of a Basic Configuration File
```
events {
    worker_connections 1024;
}

http {
    server {
        listen 80;
        server_name example.com;

        location / {
            root /var/www/html;
            index index.html;
        }
    }
}
```
* **`events { worker_connections 1024; }`**: Allows up to 1024 simultaneous connections.
* **`http { ... }`**: Starts the block for HTTP settings.
* **`server { ... }`**: Defines a server block for the website.
* **`listen 80;`**: Listens for requests on port 80.
* **`server_name example.com;`**: Specifies the domain name.
* **`location / { ... }`**: Handles requests for the root URL (/).
* **`root /var/www/html;`**: Sets the directory where website files are stored.
* **`index index.html;`**: Specifies the default file to serve (e.g., index.html).
###### Key Syntax Rules
* Semicolons (;): Every directive must end with a semicolon. For example, listen 80; (no space before the semicolon).
* Curly braces ({}): Used to open and close blocks. Make sure each opening { has a matching closing }.
* Indentation: Not required but recommended for readability (e.g., indenting nested blocks with spaces).
* Comments: Start with a # symbol. For example, # This is a comment.

#### Nginx as a Reverse Proxy
A reverse proxy is a server that sits between clients (e.g., a user’s browser) and backend servers (e.g., web application servers). It forwards client requests to the appropriate backend server and returns the server’s response to the client. This setup provides several benefits:

* Security: Hides backend servers from direct client access.
* Load Distribution: Can distribute requests across multiple servers.
* Performance: Enables caching and SSL termination.
To configure Nginx as a reverse proxy, use the proxy_pass directive within a location block to forward requests to a backend server.
```
http {
    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://backend:8080;
        }
    }
}
```
* **`proxy_pass http://backend:8080;`**: Forwards requests to the backend server at http://backend:8080. Replace backend:8080 with your actual backend server’s address or an upstream block name (e.g., backend). When proxying requests, the backend server often needs details about the original client. Use proxy_set_header to pass this information:

* **`proxy_set_header Host $host;`**: Sends the original host requested by the client.
* **`proxy_set_header X-Real-IP $remote_addr;`**: Sends the client’s real IP address.
* **`proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;`**: Sends the client’s IP and any previous proxies’ IPs.
```
location / {
    proxy_pass http://backend:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```
##### Handling Slow Clients with Proxy Buffering
For slow clients or when the Nginx server has limited memory, enable proxy buffering to store the backend’s response temporarily before sending it to the client. This reduces the load on Nginx and prevents it from holding connections open too long.
```
location / {
    proxy_pass http://backend:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    
    proxy_buffering on;
    proxy_buffer_size 8k;
    proxy_buffers 8 32k;
}
```
* **`proxy_buffering on;`**: Enables buffering of the backend response.
* **`proxy_buffer_size 8k;`**: Sets the size of the initial buffer (e.g., for headers).
* **`proxy_buffers 8 32k;`**: Defines 8 buffers of 32KB each for the rest of the response.
* Note: Adjust buffer sizes based on your server’s memory and traffic patterns. Smaller buffers save memory but may slow down transfers for large responses.

##### Specifying Network Interfaces with Proxy Bind
If your Nginx server has multiple network interfaces (e.g., multiple IP addresses), use proxy_bind to specify which interface Nginx should use to connect to the backend server.
```
location / {
    proxy_pass http://backend:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    
    proxy_buffering on;
    proxy_buffer_size 8k;
    proxy_buffers 8 32k;
    
    proxy_bind 192.168.1.100;
}
```
* **`proxy_bind 192.168.1.100;`**: Forces Nginx to use the network interface with IP 192.168.1.100 for outbound connections to the backend. Replace with your desired IP.

#### What is Load Balancing?
Load balancing is like distributing tasks among multiple workers to get things done faster and more efficiently. In web terms, it means spreading incoming user requests across multiple servers so that no single server gets overwhelmed. This helps:

* Handle more traffic.
* Improve website speed.
* Keep the website running even if one server fails.
* How Does Nginx Perform Load Balancing?
* Nginx acts as a "traffic cop" that stands in front of your backend servers (the ones running your website or app). When a user makes a request, Nginx decides which backend server should handle it based on the load balancing method you choose.

##### Types of Load Balancing Methods in Nginx
Nginx supports several methods to distribute traffic:

###### 1. Round Robin (Default):
* Requests are distributed one by one to each server in order, like dealing cards.
* Example: Request 1 goes to Server A, Request 2 to Server B, Request 3 back to Server A, and so on.
###### 2. Least Connections:
* Sends the request to the server with the fewest active connections (the least busy server). 
* Useful when servers have different capacities or when requests take varying amounts of time.
###### 3. IP Hash:
* Uses the user’s IP address to decide which server to send the request to. 
* Ensures that a user is always directed to the same server (helpful for apps that need to remember user sessions).
###### 4. Weighted Load Balancing:
* Allows you to assign a "weight" to each server. Servers with higher weights get more requests.
* Example: If Server A has a weight of 2 and Server B has a weight of 1, Server A gets twice as many requests.

##### How to Write a Load Balancing Configuration File in Nginx
To set up load balancing, you need to add an upstream block in your configuration file. This block defines the group of backend servers that Nginx will distribute traffic to.
This sets the maximum number of simultaneous connections.
```
events {
    worker_connections 1024;
}
```

Add the http block:
```
http {
```
All HTTP-related settings, including load balancing, go inside this block.

Define the upstream block inside http:
```
    upstream backend {
        server backend1.example.com;
        server backend2.example.com;
    }
```

* **`upstream backend`**: Names the group of backend servers (you can choose any name).
* **`server backend1.example.com;`**: Lists the first backend server (replace with your server’s address).
* **`server backend2.example.com;`**: Lists the second backend server.
* Note: By default, Nginx uses round-robin load balancing.

Configure the server block inside http:
```
    server {
        listen 80;
        location / {
            proxy_pass http://backend;
        }
    }

``` 
* **`server { ... }`**: Defines how Nginx listens for incoming requests.
* **`listen 80;`**: Listens on port 80.
* **`location / { ... }`**: Handles requests for the root URL.
* **`proxy_pass http://backend;`**: Forwards requests to the backend upstream group.
* Important: Include http:// before the upstream name.
Close the http block:
```
}
```
```
events {
    worker_connections 1024;
}

http {
    upstream backend {
        server backend1.example.com;
        server backend2.example.com;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://backend;
        }
    }
}
```
##### Where to Save the Configuration File
For standard Nginx:
* Save the file in /etc/nginx/conf.d/, e.g., /etc/nginx/conf.d/loadbalancer.conf.

For Docker:
* Save the file in a directory on your computer, e.g., /path/to/my-loadbalancer/nginx.conf.
* When running the Docker container, mount the file using -v /path/to/my-loadbalancer/nginx.conf:/etc/nginx/nginx.conf.


## Git

### What is Git
Git is a tool that helps you keep track of changes to your files, especially when working on projects like coding or writing. Imagine it as a "time machine" for your work—it lets you save different versions of your files so you can go back if something goes wrong. It’s super popular with programmers, but anyone can use it to manage file changes.

#### Why is Git Useful?
**`Track Changes`**: Git saves every change you make, so you know what happened, when, and who did it.

**`Teamwork`**: It lets many people work on the same project without messing up each other’s stuff.

**`Backup`**: Your work is stored safely in multiple places.

**`Try New Ideas`**: You can experiment without ruining the main project.

#### Repository (Repo): 
A folder with all your project files and their history—like a home for your work.

#### Commit: 
A saved version of your files at a certain time, like a snapshot.

#### Branch: 
A separate version of your project where you can work on new stuff without touching the main part.

#### Merge:
Mixing changes from one branch into another.

### Basic Git Commands and What They Do
Here are the most important Git commands you’ll use, explained simply:

#### git init
* What it does: Starts a new Git repository in your folder.

* When to use it: When you begin a new project and want Git to track it.

* Example: Type git init in your project folder to set it up.
#### git add <file>

* What it does: Adds a file to the "staging area" (like a prep zone) before saving it.

* Tip: Use git add . to add all changed files at once.

* Example: git add index.html prepares that file for a commit.

#### git commit -m "message"

* What it does: Saves your changes as a commit with a note (message) about what you did.

* Example: git commit -m "Added a new button" saves your work with that description.

#### git status
* What it does: Shows what’s going on—like which files have changed or are ready to save.

* When to use it: To check your project’s status before committing.

#### git log
* What it does: Lists all your commits with their messages and dates.

* When to use it: To look at your project’s history.

#### git branch
* What it does: Shows all branches in your repository.

* Tip: Use git branch new-feature to make a new branch called "new-feature".

#### git checkout <branch>
* What it does: Switches you to another branch to work on it.

* Example: git checkout new-feature takes you to that branch.

#### git merge <branch>
* What it does: Brings changes from one branch into the one you’re on.

* Example: git merge new-feature adds those changes to your current branch.

#### git pull
* What it does: Grabs updates from an online (remote) repository and adds them to your project.

* When to use it: To get the latest changes from your team.

#### git push
* What it does: Sends your commits to an online repository so others can see them.

* When to use it: After committing, to share your work.

### Working with Remote Repositories
Git often teams up with online platforms like GitHub or GitLab. These "remote repositories" let people work together. Here’s how:

#### Clone: 
Copy a remote repository to your computer with git clone <url>.
#### Push: 
Upload your changes with git push.
#### Pull: 
Download updates with git pull.

#### The Staging Area (or Index)
The Staging Area (also called the "Index") is a core concept in Git. It's an intermediate step between your Working Directory (your local files) and your Commit History (the .git repository).

Think of it like a prep table in a kitchen. You gather and prepare all your ingredients (your code changes) on the table before you put them in the oven (commit them). This allows you to carefully craft your commit, including only the specific changes you want, rather than committing everything you've worked on at once.

The basic workflow is:

* Working Directory: You modify files.

* **`git add <file>`**: You move specific changes from your Working Directory to the Staging Area.

* **`git commit`**: Git takes the files in the Staging Area and permanently stores them as a snapshot in your Commit History.


#### git revert: The Safe Way to Undo
git revert is a safe, forward-moving command used to undo the changes introduced by a specific commit. It doesn't delete or alter the original commit; instead, it creates a brand new commit that applies the inverse changes.

##### How It Works

When you revert a commit, Git examines the changes in that commit and creates a new commit that does the exact opposite. If the original commit added a line of code, the revert commit will remove that line. The original "bad" commit remains in the project history, and a new "revert" commit is added after it.

##### Key Characteristics

Doesn't Rewrite History: This is its main advantage. Because it creates a new commit, it's a non-destructive action and is safe to use on public, shared branches (like main or develop).

Preserves History: It maintains a clear and transparent record of the project, showing that a change was made and then later undone.

Requires a New Commit: You will be prompted for a commit message for the revert commit.

##### When to Use It

Use git revert when you need to undo a commit that has already been pushed to a shared repository.

Example
Imagine you want to undo the changes from a commit with the hash a8e5fdf:

This will create a new commit that undoes the changes from a8e5fdf
```
git revert a8e5fdf
```
* Git will open your editor to confirm the commit message for the new revert commit.

* After saving, the new commit will be added to your branch.

#### git reset: Moving and Discarding Commits (Local Use Only)
git reset is a powerful and versatile command for undoing changes. Unlike revert, it rewrites commit history by moving the current branch's HEAD pointer to a different commit, effectively discarding any commits that came after it.

The Golden Rule: Never use git reset on commits that have been pushed to a public/shared branch. Only use it to clean up your local commit history.

git reset operates on three "trees" in Git: the Commit History, the Staging Area (Index), and the Working Directory. The command has three primary modes that affect these trees differently.

##### **`git reset --soft`**
This is the most gentle mode. It moves the HEAD pointer to the specified commit but does not touch the Staging Area or the Working Directory.

Effect: The commits are gone from the history, but all the changes from those commits are now in your Staging Area, ready to be committed again.
Use Case: Squashing multiple local commits into a single, clean commit.
Example: To combine the last 3 commits into one:
Bash

Resets the branch pointer back 3 commits, but leaves all changes staged
```
git reset --soft HEAD~3
```
Now, all changes are staged. You can create a single new commit.
```
git commit -m "This is the new, combined commit message"
```
##### **`git reset --mixed (Default Mode)`**

This mode moves the HEAD pointer and also resets the Staging Area. It does not touch the Working Directory.

Effect: The commits are gone, and the changes are no longer staged. However, the modifications are still present in your local files (your Working Directory).

Use Case: To un-commit and un-stage changes. You decided you want to rework the changes before committing them again.

Example: To undo your last commit but keep the changes:
```
git reset --mixed HEAD~1
```

##### **`git reset --hard`**
This is the most powerful and destructive mode. It moves the HEAD pointer and resets both the Staging Area and the Working Directory to match the specified commit.

Effect: The commits are gone, and all changes in those commits are permanently deleted from your files. Any uncommitted work is lost.
Use Case: To completely throw away the last few commits and all the work associated with them.
Example: To permanently delete the last two commits and all their changes:
Bash

WARNING: This action cannot be easily undone.
```
git reset --hard HEAD~2
```

#### **`git rebase: Creating a Cleaner, Linear History`** 🧹
git rebase is another command that rewrites history. Its primary purpose is to integrate changes from one branch onto another by moving a sequence of commits to a new base commit. The result is a perfectly linear history, free of merge commits.

##### How It Works
Instead of creating a merge commit, git rebase "re-plays" the commits from your feature branch on top of the target branch (e.g., main). It goes to the common ancestor, saves the changes you made in your branch as temporary patches, moves your branch to the tip of the target branch, and then applies the patches one by one, creating new commits with new hashes.

##### Key Characteristics

Rewrites History: Because it creates new commits, it's a destructive operation.

Creates a Linear History: It avoids the "Merge branch 'main'" commits, making the project history much easier to read and understand.

Can Cause Conflicts: You may have to resolve conflicts for each commit that is being re-played.

##### The Golden Rule of Rebasing

Just like git reset, NEVER rebase a branch that has been shared with others. If other developers have based their work on your original commits, your rebased branch will cause massive confusion and conflicts for them.

##### When to Use It
Use git rebase to update your local feature branch with the latest changes from the main branch before you merge it. This ensures a clean "fast-forward" merge.

Example
You are on your feature branch and want to incorporate the latest updates from the main branch.
```
#witch to main and pull the latest changes
git switch main
git pull origin main
#Switch back to your feature branch
git switch feature

#Rebase your feature branch commits on top of the latest main
git rebase main
#Your entire feature branch is now moved to the tip of main, and its history is linear.
```

| Command     | Purpose                                    | Effect on History                                   | Primary Use Case                                                   |
|-------------|--------------------------------------------|-----------------------------------------------------|---------------------------------------------------------------------|
| `git revert` | Safely undo a commit                       | Creates a new commit. Does not rewrite history.     | Undoing a commit on a public/shared branch.                        |
| `git reset`  | Move the branch pointer, discarding commits | Rewrites history. Can delete commits and changes.   | Cleaning up your local commit history before sharing.              |
| `git rebase` | Move a sequence of commits to a new base   | Rewrites history. Creates new commits to make history linear. | Integrating upstream changes into your local feature branch. |

### .gitignore

A .gitignore file is a text file that tells Git which files or directories to intentionally ignore. Ignored files will not be tracked, meaning they won't be staged or committed. This is essential for keeping your repository clean and avoiding accidental check-ins of unwanted files.

#### Common files to ignore:

* **`Dependencies`** (node_modules/, vendor/)

* **`Compiled code`** (build/, dist/, *.class)

* **`Logs and temporary files`** (*.log, tmp/)

* **`System files`** (.DS_Store, Thumbs.db)

* **`Local environment and secret files`** (.env, secrets.yml)

### Resolving Merge Conflicts

A merge conflict occurs when Git cannot automatically resolve differences in code between two branches. This usually happens when two developers have edited the same lines in the same file.

When a conflict happens, Git will pause the merge and mark the conflicting sections in the file with markers: <<<<<<<, =======, and >>>>>>>.

#### How to resolve a conflict:

* Run git status to see which files have conflicts.

* Open the conflicted file(s) in your editor.

* Manually edit the file to remove the markers and decide which code to keep (it could be your version, the other version, or a combination of both).

* Once you've fixed the file, save it and use git add <file> to mark the conflict as resolved.

* When all conflicts are resolved and staged, run git commit to finalize the merge.

### Remote-Tracking Branches (e.g., origin/main)

A remote-tracking branch is a local, read-only reference to the state of a branch on a remote repository. For example, origin/main is your local copy of the main branch from the origin remote.

These branches are updated when you run git fetch. They are useful because they allow you to see the progress of the remote repository without having to immediately merge those changes into your local branches. You can compare your local branch to the remote-tracking branch (git diff main origin/main) to see what has changed.

### Git Hooks
Git hooks are scripts that run automatically at certain points in the Git lifecycle, such as before a commit, after a push, or before a merge. They are a powerful way to automate tasks and enforce team standards.

A common hook is pre-commit. This hook runs before a commit message is even typed. It's often used to:

* Run a code linter to check for style issues.

* Run a code formatter (like Prettier or Black).

* Check for debug statements or secrets that shouldn't be committed. If the pre-commit script exits with an error, the commit is aborted.

### Git Tagging for Releases

As seen in your CI/CD pipeline, Git tags are crucial for marking release points. A tag is a pointer to a specific commit, used to capture a point in history as being important.

* **`Lightweight Tags`**: A simple pointer to a commit.

* **`Annotated Tags`**: A full object in the Git database that includes the tagger's name, email, date, and a tagging message. Annotated tags are recommended for releases.

Example commands:

```
# Create an annotated tag for version 1.0.0
git tag -a v1.0.0 -m "Release version 1.0.0"

# Tags are not pushed by default. You must push them explicitly.
# This pushes all of your local tags to the remote repository.
git push --tags
```

## What is GitLab?

GitLab is a web-based platform that provides a complete DevOps solution, enabling teams to plan, build, secure, and deploy software efficiently. It is built on Git, a distributed version control system, and offers tools to manage the entire software development lifecycle. GitLab can be hosted on your own servers (self-hosted) or used as a cloud-based service, making it flexible for teams and organizations of any size.

### What Are the Uses of GitLab?

GitLab serves several key purposes in software development:

* **`Version Control`**: It allows developers to track and manage changes to code, keeping a history of modifications. This ensures collaboration without losing earlier versions.

* **`Issue Tracking`**: Teams can create and manage tasks, bugs, or feature requests, keeping projects organized.

* **`CI/CD Automation`**: GitLab automates building, testing, and deploying code, speeding up software releases.

* **`Code Review`**: It provides tools to review code changes, ensuring quality and consistency.

* **`Project Management`**: Features like milestones and boards help plan and track progress.

* **`Security and Compliance`**: GitLab includes tools to scan for vulnerabilities and enforce standards.

For example, a team might use GitLab to store a mobile app’s code, track a reported crash, and deploy a fix automatically.

### What Are the Advantages of GitLab?
Using GitLab offers numerous benefits:

* **`All-in-One Platform`**: It integrates tools for coding, tracking, and deployment into one interface, simplifying workflows.

* **`Open-Source`**: Its open-source nature allows customization, such as adding unique features.

* **`Scalability`**: It works for small startups and large enterprises alike.

* **`Security`**: Features like access controls and vulnerability scanning protect code and data.

* **`Collaboration`**: Tools like merge requests and comments improve teamwork.

For instance, a company might use GitLab to securely collaborate on a confidential project while automating repetitive tasks.

### What Happens If GitLab Is Not Used in a System?
Without GitLab or a similar tool, teams face challenges:

* **`No Version Control`**: Code changes are hard to track, risking errors like overwriting work.

* **`Manual Processes`**: Testing and deployment become slow and error-prone without automation.

* **`Poor Collaboration`**: Lack of a shared platform leads to miscommunication or delays.

* **`Security Risks`**: Without access controls, sensitive code could be exposed.

For example, a team might accidentally deploy broken code without a system to test changes first.

### Groups
A group is a way to organize related projects and users under one namespace. For example, a university might create a group called “CS_Department” for computer science projects.

Creating a Group:

1. Go to the “Groups” menu and click “Create group.”

2. Name: Enter a unique name, e.g., “CS_Department.”

3. Description: Add a brief purpose, e.g., “Computer science research projects.”

4. Visibility Level: Choose who can see it:

* **`Private`**: Only group members can access it (e.g., for confidential research).

* **`Internal`**: All logged-in users on the instance (except external users) can see it (e.g., for university-wide projects).

* **`Public`**: Anyone, even without an account, can view it (e.g., for open-source work).

### Subgroups
A subgroup is a group nested within another group, allowing hierarchical organization of projects and permissions. Introduced in GitLab 9.0, subgroups support up to 20 levels of nesting and inherit permissions from their parent group unless overridden.

#### How Subgroups Differ from Groups and Projects:

##### Compared to a Group

A group is a top-level or parent entity, while a subgroup is a child group within it, offering finer structure. For example, an “Engineering” group might have “Frontend” and “Backend” subgroups.
##### Compared to a Project: 

A project holds code and resources, while a subgroup is a container for multiple projects or subgroups, used for organization.

#### Why Create Subgroups?

1. Organize large projects into sub-components (e.g., “Design” and “Engineering” under “Product_Development”).

2. Manage permissions for specific teams (e.g., a private “Security” subgroup).

3. Separate internal/external work with different visibility levels.

4. Improve navigation in large organizations.

#### Creating a Subgroup:

1. Go to the parent group’s overview page and click “New subgroup.”

2. Name: Enter a unique name, e.g., “Frontend.”

3. Description: Add a purpose, e.g., “Frontend development projects.”

4. Visibility Level: Choose Private, Internal, or Public (must align with or be more restrictive than the parent group’s 
visibility).

5. Click “Create subgroup.”

6. Permissions: Requires at least the Maintainer role on the parent group.


### Projects
A project is a repository that holds code, issues, and resources. It belongs to a user or group.

1. Go to “Projects” and click “Create a project.”

2. Name: Enter a name, e.g., “WebsiteUpdate.”

3. Description: Add details, e.g., “Updating the university website.”

4. Visibility Level: Select Private, Internal, or Public (same as groups).

5. Initialize with README: Optionally, add a README file to start documentation.


### Users
Users are individuals with GitLab accounts who work on groups and projects.

1. In a group or project, go to “Members” in the Settings menu.

2. Invite by Email: Enter the user’s email (e.g., “student@university.com”).

3. Select Role: Assign a role (see “Roles” below).

4. Access Level: Choose group or project access.

5. Click “Invite.”


#### Roles
Roles define a user’s permissions in a group or project. GitLab has five main roles:

1. **`Guest`**: Can view issues and comment but not edit code.

2. **`Reporter`**: Can view code, create issues, and comment but not push code.

3. **`Developer`**: Can push code, create branches, and manage issues but not configure settings.

4. **`Maintainer`**: Can manage settings, merge branches, and add members but not delete projects.

5. **`Owner`**: Full control, including deleting projects and managing everything.

#### Visibility Levels Explained
When setting up groups or projects, you choose a visibility level:

1. **`Private`**: Only added members can access it. Example: A private project for a secret app.

2. **`Internal`**: Visible to all logged-in users on the instance (except external users). Example: An internal HR group for employees.

3. **`Public`**: Open to everyone, even without an account. Example: A public library for global use.


### Issues
Issues track tasks, bugs, or requests.

1. In a project, go to “Issues” and click “New issue.”

2. Title: E.g., “Fix login error.”

3. Description: Detail the problem.

4. Assignee: Assign to a user.

5. Labels: Add tags like “bug” or “urgent.”

6. Milestone: Link to a deadline.

7. Click “Create issue.”

### Merge Requests
Merge Requests (MRs) propose and review code changes.

1. Go to “Merge Requests” and click “New merge request.”

2. Source Branch: Select the changed branch.

3. Target Branch: Choose “main” or another branch.

4. Title and Description: Explain the changes.

5. Assignees and Reviewers: Add team members.

6. Click “Create merge request.”

### CI/CD
CI/CD automates code building, testing, and deployment.

1. In the project, go to “CI/CD” settings.

2. Create a .gitlab-ci.yml file in the repository.

3. Define jobs (e.g., “build,” “test,” “deploy”).

4. Use GitLab runners to execute jobs.

### Wiki
Wiki stores project documentation.

1. Go to “Wiki” in the project menu.

2. Click “Create your first page.”

3. Title: E.g., “Setup Guide.”

4. Content: Write using Markdown.

5. Click “Save.”

### Snippets
Snippets share small code or text pieces.

1. Go to “Snippets” and click “New snippet.”

2. Title: E.g., “Config Script.”

3. Content: Paste the code.

4. Visibility: Choose Public or Private.

5. Click “Create snippet.”

### Container Registry
Container Registry stores Docker images.

1. Go to “Container Registry” in the project.

2. Push an image using Docker commands.

3. Pull images for deployment.

### Packages
Packages manage libraries like npm or Maven.

1. Go to “Packages & Registries.”

2. Choose a package type (e.g., npm).

3. Publish using provided commands.

### Security & Compliance
Security & Compliance scans code for issues.

Enabling Scans:

1. Go to “Security & Compliance.”

2. Enable SAST, DAST, or Dependency Scanning in CI/CD settings.


### Operations
Operations manages deployments and monitoring.

1. Go to “Operations” > “Environments.”

2. Define “staging” or “production.”

3. Monitor with metrics.

### Analytics
Analytics provides project insights.

1. Go to “Analytics.”

2. View Contribution, Cycle, or Code Review Analytics.

### Settings
Settings configures groups/projects.

1. Go to “Settings.”

2. Adjust General, Members, Integrations, or Webhooks.

### Additional Features

1. **`Feature Flags`**: Test features with specific users (e.g., “new_ui” for 10% of users).

2. **`Boards`**: Visualize issues/epics (e.g., “To Do” to “Done”).

3. **`Milestones`**: Set deadlines (e.g., “Version 1.0”).

4. **`Time Tracking`**: Log time on tasks (e.g., 2 hours on a bug).

5. **`Roadmaps`**: Plan timelines (e.g., next quarter’s features).

6. **`GitLab Pages`**: Host static sites (e.g., documentation).

7. **`Web IDE`**: Edit code in-browser (e.g., fix a typo).

8. **` Code Review Tools`**: Comment on MRs (e.g., suggest changes).

9. **`GitLab Duo`**: AI tools for code (e.g., generate a test).

```
git init
```
```
git add .
```
```
git commit -m "devops"
```
```
git branch -M main
```
```
git remote add origin http://
```
```
git push -uf origin main
```


#### Explain every detail of CICD pipeline

```
stages:
  - build
  - pre-security
  - deploy
  - build-develop
  - deploy-develop
variables:
  NEXUS_REGISTRY: localhost:8083
  IMAGE_NAME: simple-app

build: 
  stage: build
  rules:
    - if: $CI_COMMIT_TAG
  script:
    - docker login -u $NEXUS_USERNAME -p $NEXUS_PASSWORD $NEXUS_REGISTRY
    - docker build -t $NEXUS_REGISTRY/$IMAGE_NAME:$CI_COMMIT_REF_NAME .
    - docker push $NEXUS_REGISTRY/$IMAGE_NAME:$CI_COMMIT_REF_NAME
  tags:
    - sami-runner 

security:
   stage: pre-security
   rules:
     - if: $CI_COMMIT_TAG
   script:
     - trivy image --exit-code 0 --severity CRITICAL  $NEXUS_REGISTRY/$IMAGE_NAME:$CI_COMMIT_REF_NAME   
   tags:
     - sami-runner      
 
deploy-for-product:
  stage: deploy
  rules:
    - if: $CI_COMMIT_TAG
  when: manual  
  script:
    - EXISTING=$(docker ps -qa -f name=app1) && [ -n "$EXISTING" ] && docker stop "$EXISTING" && docker rm "$EXISTING"
    - docker run -d --name app1 -p 5000:5000  $NEXUS_REGISTRY/$IMAGE_NAME:$CI_COMMIT_REF_NAME
  tags:
    - sami-runner

build-for-develop:
  stage:  build-develop
  rules:
    - if: $CI_PIPELINE_SOURCE == "push" && $CI_COMMIT_BRANCH == "DEV"
  script:
    - docker login -u $NEXUS_USERNAME -p $NEXUS_PASSWORD $NEXUS_REGISTRY
    - docker build -t $NEXUS_REGISTRY/$IMAGE_NAME:dev
    - docker push   $NEXUS_REGISTRY/$IMAGE_NAME:dev
  tags:
    - sami-runner  

deploy-for-develop:
  stage: deploy-develop
  rules:
    - if: $CI_PIPELINE_SOURCE == "push" && $CI_COMMIT_BRANCH == "DEV"
  script:
    -  EEXISTING=$(docker ps -qa -f name=app1) && [ -n "$EXISTING" ] && docker stop "$EXISTING" && docker rm "$EXISTING"
    -  docker run --name app-dev -p 5001:5000 $NEXUS_REGISTRY/$IMAGE_NAME:dev
  tags:
    - sami-runner  

```
##### 1. stages
This section defines the sequence of stages in the pipeline. Jobs assigned to the same stage can run in parallel, but stages themselves run in the order they are listed.
```
stages:
  - build
  - pre-security
  - deploy
  - build-develop
  - deploy-develop

```

**`build`**: Compiles and packages the application into a Docker image for production.

**`pre-security`**: Runs security scans on the production Docker image before deployment.

**`deploy`**: Deploys the production image to the server.

**`build-develop`**: Builds a Docker image for the development environment.

**`deploy-develop`**: Deploys the development image to the server.

##### 2. variables

This section declares global variables that are used across multiple jobs in the pipeline.
```
variables:
  NEXUS_REGISTRY: localhost:8083
  IMAGE_NAME: simple-app
```

**`NEXUS_REGISTRY`**: localhost:8083: Defines the URL of the Nexus Docker registry where images will be stored and pulled from.

**`IMAGE_NAME`**: simple-app: Specifies the base name for the Docker image being built.

##### 3. Production Workflow (Triggered by Git Tags)

This workflow is initiated whenever a new Git tag is pushed to the repository. It is intended for creating and deploying a production release.

Job: build
This job builds the production Docker image and pushes it to the Nexus registry.
```
build: 
  stage: build
  rules:
    - if: $CI_COMMIT_TAG
  script:
    - docker login -u $NEXUS_USERNAME -p $NEXUS_PASSWORD $NEXUS_REGISTRY
    - docker build -t $NEXUS_REGISTRY/$IMAGE_NAME:$CI_COMMIT_REF_NAME .
    - docker push $NEXUS_REGISTRY/$IMAGE_NAME:$CI_COMMIT_REF_NAME
  tags:
    - sami-runner
```

**`stage`**: build: Assigns this job to the build stage.

**`rules: - if: $CI_COMMIT_TAG`**: This rule ensures the job only runs when the pipeline is triggered by a Git tag. $CI_COMMIT_TAG is a predefined GitLab CI/CD variable that contains the tag name.

**`script`**: A sequence of shell commands executed by the GitLab Runner.
* **`docker login ...`**: Logs into the specified Nexus registry using credentials stored in predefined GitLab CI/CD variables ($NEXUS_USERNAME, $NEXUS_PASSWORD). These should be configured in your project's CI/CD settings.

* **`docker build ...`**: Builds a new Docker image.

* **`-t $NEXUS_REGISTRY/$IMAGE_NAME:$CI_COMMIT_REF_NAME`**: Tags the image. The tag is composed of the registry URL, the image name, and the Git tag that triggered the pipeline (e.g., v1.0.0). The . at the end specifies that the Docker build context is the current directory.

* **`docker push ...`**: Pushes the newly built and tagged image to the Nexus registry.
* **`tags`**: - sami-runner: Specifies that this job must be executed by a GitLab Runner that has the sami-runner tag.

##### 4. security
This job performs a vulnerability scan on the production image.
```
security:
   stage: pre-security
   rules:
     - if: $CI_COMMIT_TAG
   script:
     - trivy image --exit-code 0 --severity CRITICAL  $NEXUS_REGISTRY/$IMAGE_NAME:$CI_COMMIT_REF_NAME   
   tags:
     - sami-runner
```

**`stage`**: pre-security: Assigns this job to the pre-security stage, which runs after build.

**`rules`**: - if: $CI_COMMIT_TAG: Same rule as the build job, runs only for Git tags.

**`trivy image ...`**: Uses the Trivy security scanner to check the image for vulnerabilities.

**`--exit-code 0`**: Ensures the pipeline job will succeed even if vulnerabilities are found. This is useful for auditing without blocking the pipeline. To enforce security, you could set it to 1 to fail the job on detected issues.

**`--severity CRITICAL`**: Filters the scan to only report vulnerabilities of CRITICAL severity.

**`tags`**: - sami-runner: Uses a runner tagged sami-runner.

##### 5. deploy-for-product
This job deploys the production image as a running container. It requires manual approval.
```
deploy-for-product:
  stage: deploy
  rules:
    - if: $CI_COMMIT_TAG
  when: manual  
  script:
    - EXISTING=$(docker ps -qa -f name=app1) && [ -n "$EXISTING" ] && docker stop "$EXISTING" && docker rm "$EXISTING"
    - docker run -d --name app1 -p 5000:5000  $NEXUS_REGISTRY/$IMAGE_NAME:$CI_COMMIT_REF_NAME
  tags:
    - sami-runner
```

**`stage`**: deploy: Assigns this job to the deploy stage.

**`rules`**: - if: $CI_COMMIT_TAG: Runs only for Git tags.

**`when: manual`**: This is a critical keyword. It pauses the pipeline and requires a user to manually trigger this job from the GitLab UI. This is a safety measure to prevent accidental deployments to production.

**`EXISTING=$(...)`**: This line checks if a container named app1 is already running. If it exists ([ -n "$EXISTING" ]), the script stops (docker stop) and removes (docker rm) it to ensure a clean deployment.

**`docker run ...`**: Starts a new container.

**`-d`**: Runs the container in detached mode (in the background).

**`--name app1`**: Assigns the name app1 to the container.

**`-p 5000:5000`**: Maps port 5000 on the host machine to port 5000 inside the container.

**`tags`**: - sami-runner: Uses a runner tagged sami-runner.

##### 6. build-for-develop

This job builds the development Docker image.
```
build-for-develop:
  stage:  build-develop
  rules:
    - if: $CI_PIPELINE_SOURCE == "push" && $CI_COMMIT_BRANCH == "DEV"
  script:
    - docker login -u $NEXUS_USERNAME -p $NEXUS_PASSWORD $NEXUS_REGISTRY
    - docker build -t $NEXUS_REGISTRY/$IMAGE_NAME:dev .
    - docker push $NEXUS_REGISTRY/$IMAGE_NAME:dev
  tags:
    - sami-runner
```

**`stage`**: build-develop: Assigns this job to the build-develop stage.

**`rules: - if`**: ...: This rule ensures the job runs only when a push event occurs on the DEV branch.

**`$CI_PIPELINE_SOURCE`** == "push": Checks if the trigger was a git push.

**`$CI_COMMIT_BRANCH`** == "DEV": Checks if the push was to the DEV branch.

**`docker login ...`**: Logs into the Nexus registry.

**`docker build ...`**: Builds a Docker image and tags it with a static tag: dev. Correction: Added . at the end to specify the build context.

**`docker push ...`**: Pushes the dev image to the Nexus registry. Correction: The image name and tag were added to this command.

**`tags`**: - sami-runner: Uses a runner tagged sami-runner.

##### 7. deploy-for-develop (Corrected Version)
This job deploys the development image.
```
deploy-for-develop:
  stage: deploy-develop
  rules:
    - if: $CI_PIPELINE_SOURCE == "push" && $CI_COMMIT_BRANCH == "DEV"
  script:
    -  EXISTING=$(docker ps -qa -f name=app-dev) && [ -n "$EXISTING" ] && docker stop "$EXISTING" && docker rm "$EXISTING"
    -  docker run -d --name app-dev -p 5001:5000 $NEXUS_REGISTRY/$IMAGE_NAME:dev
  tags:
    - sami-runner
```

**`stage`**: deploy-develop: Assigns this job to the deploy-develop stage.

**`rules: - if: ...`**: Same rule as build-for-develop, runs on pushes to the DEV branch.

**`EXISTING=$(...)`**: Checks for, stops, and removes any pre-existing container named app-dev. Correction: Fixed a typo (EEXISTING to EXISTING) and changed the filter name=app1 to name=app-dev to match the container being deployed in this job.

**`docker run ...`**: Starts the development container.

**`--name app-dev`**: Names the container app-dev.

**`-p 5001:5000`**: Maps port 5001 on the host to port 5000 in the container, preventing a port conflict with the production container (app1).

**`tags`**: - sami-runner: Uses a runner tagged sami-runner.

### GitLab & CI/CD Documentation Additions

#### 1. Job Artifacts

In GitLab CI/CD, artifacts are files and directories that are generated by a job. After the job finishes, GitLab saves these artifacts and makes them available for download in the UI. More importantly, they can be passed to jobs in later stages of the same pipeline.

##### Artifact Use Case:

* A build job compiles your application and creates a binary file.

* You save this binary as an artifact.

* A deploy job in a later stage can download this artifact and deploy it to a server, ensuring you are deploying the exact same file that was built and tested.

Example in .gitlab-ci.yml:
```
build-job:
  stage: build
  script:
    - ./build_script.sh # This script creates a binary in the ./bin directory
  artifacts:
    paths:
      - bin/my-app # This path is saved as an artifact
    expire_in: 1 week
```

#### 2. Runner Security
GitLab Runners execute the code in your pipeline, so their security is critical.

* **`Principle of Least Privilege`**: A runner should only have the permissions it absolutely needs to do its job.

* **`Avoid Privileged Mode`**: When using Docker executors, avoid running containers in privileged mode unless it is strictly necessary (e.g., for building Docker images with Docker-in-Docker). Privileged mode gives the container full root access to the host machine, which is a major security risk.

* **`Dedicated Runners`**: For sensitive projects, use dedicated runners that are not shared with other projects to prevent potential data leakage.

#### 3. Environment Variables & Secrets Management
Never hardcode credentials, tokens, or other secrets directly in your .gitlab-ci.yml file. GitLab has a secure place to store these:

Project > Settings > CI/CD > Variables

Here, you can define variables that are securely passed to the runner at runtime.

* Protected: A protected variable is only passed to pipelines running on protected branches (like main).

* Masked: A masked variable's value will be hidden in job logs to prevent accidental exposure.

#### 4. Pipeline Visualization
GitLab provides a real-time, visual representation of your entire pipeline. This pipeline graph shows all your stages and the jobs within them. You can easily see which jobs are running, which have passed, and which have failed. Clicking on a failed job will take you directly to its log, making it an essential tool for quickly diagnosing and debugging pipeline issues.

#### 5. Merge Request Pipelines
A Merge Request (MR) Pipeline is a pipeline that runs automatically whenever a new MR is created or an existing MR is updated with new commits. This is a powerful feature that acts as a quality gate.

You can configure it to run tests, linters, and security scans on the proposed changes. This ensures that code meets quality standards before it is merged into a key branch like main or develop, preventing broken code from entering the main codebase.

#### 6. Caching Dependencies
Many projects rely on dependencies that need to be downloaded (e.g., from npm, Maven, or PyPI). Downloading these on every pipeline run is slow and inefficient. GitLab's cache keyword allows you to save these dependencies between pipeline runs.

The first time a job runs, it downloads the dependencies and saves them in a cache. Subsequent runs of the same job will find the cache and restore the files, dramatically speeding up the pipeline.

Example for a Node.js project:
```
build-node-app:
  stage: build
  image: node:18
  cache:
    key:
      files:
        - package-lock.json # The cache is invalidated only if this file changes
    paths:
      - node_modules/
  script:
    - npm install
    - npm run build
```
# Ansible
## What is Ansible?
Ansible is an open-source IT automation engine that automates tasks such as configuration management, application deployment, intra-service orchestration, and provisioning. It is designed to be simple, human-readable, and powerful. Unlike other management tools, Ansible follows an 

agentless architecture, meaning it does not require any special software (agents) to be installed on the machines it manages. Instead, it uses standard, secure protocols like SSH for Linux/Unix systems and WinRM for Windows to communicate with them.


At its core, Ansible uses 

Playbooks, which are files written in YAML (a simple data-format language), to describe the desired state of your systems. This approach is 

declarative; you define what you want the system to look like, and Ansible handles the steps to get it there.

## Why Do We Need Ansible?
In modern IT, managing servers manually is inefficient and prone to errors. Imagine needing to update a security patch or configure a web server across hundreds or even thousands of servers. This challenge is where Ansible shines.

## What happens if we don't use a tool like Ansible?

* Manual Toil: System administrators spend countless hours performing repetitive tasks, such as installing software, updating configurations, and managing users.

* Inconsistency (Configuration Drift): When servers are configured manually, small differences inevitably appear over time. This "configuration drift" makes the environment unpredictable and hard to debug.

* Slow Deployments: Rolling out new applications or updates is a slow, manual, and risky process.

* Lack of Documentation: The state of the infrastructure exists only in the minds of the people who built it. There is no clear, executable record of how systems are configured.

* Ansible solves these problems by turning your infrastructure management into code. By defining configurations in playbooks, you create a single source of truth that is repeatable, verifiable, and version-controlled.

## Key Advantages and Uses of Ansible
Ansible's power comes from its simplicity and versatility. Its primary uses include:

* Configuration Management: Enforce consistent configurations across every server in your environment. For example, you can ensure that Nginx is installed, configured, and running on all your web servers with a single command.


* Application Deployment: Automate the process of deploying your applications. A playbook can handle everything from pulling the latest code from Git, installing dependencies, and restarting services to get your application live.

* Orchestration: Coordinate complex, multi-tier workflows. For instance, you can orchestrate a deployment that first sets up a database, then deploys a backend API that connects to it, and finally configures a web server and load balancer in front of it, all in the correct sequence.

* Infrastructure Provisioning: Automate the creation of infrastructure like virtual machines, cloud instances, and networks.

## The main advantages that make Ansible a popular choice are:


* Simple and Human-Readable: Playbooks are written in YAML, which is easy to learn and read, making it accessible even to those who are not full-time programmers.

* Agentless: The lack of agents simplifies setup and reduces the security footprint and resource overhead on managed nodes.

* Idempotent: Ansible playbooks are idempotent by design. This means you can run them multiple times, and they will only make changes if the system is not in the desired state. This ensures stability and predictability.

* Powerful and Flexible: It comes with thousands of pre-built modules for managing packages, services, files, cloud resources, network devices, and more.

* Acts as Documentation: An Ansible playbook is a form of executable documentation for your system's configuration.

## Core Components of Ansible
To understand how Ansible works, it's helpful to know its main components:

* **`Control Node`**: The machine where Ansible is installed and from which you run your commands and playbooks. A control node cannot be a Windows machine.

* **`Managed Nodes`**: The servers or devices that Ansible manages (e.g., web servers, database servers).

* **`Inventory`**: A file (commonly in INI or YAML format) that lists all the managed nodes. You can group hosts (e.g.,[webservers], [dbservers]) to run tasks on specific sets of machines.

* **`Playbooks`**: YAML files that define a set of tasks to be executed on managed nodes. A playbook maps a group of hosts to a set of tasks.


* **`Tasks`**: An individual action that Ansible performs, such as installing a package or starting a service. Each task calls an Ansible module.

* **`Modules`**: The core of Ansible. These are the pre-written scripts that get executed for each task. For example, the apt module manages packages on Debian/Ubuntu systems, and the copy module transfers files to the managed node.

* **`Roles`**: A structured way to organize and reuse Ansible content (tasks, handlers, variables, templates) into a portable package. For instance, you could have a role for setting up Nginx that you can reuse across multiple projects.

* **`Handlers`**: Special tasks that only run when "notified" by another task. They are typically used to restart services only if a configuration file has changed.

## Part 1: Setting up SSH Key-Based Authentication
Before using Ansible, you should configure passwordless SSH access from your control node (your PC) to the managed nodes (your servers). This is more secure than using passwords and is essential for automation. It works using a pair of cryptographic keys: a private key that you keep secret on your machine, and a public key that you place on the servers you want to manage.

Think of the public key as a lock you put on the server's door, and the private key as the only key that can open that lock.

### Step 1.1: How to Generate an SSH Key Pair
If you don't already have an SSH key pair, you can generate one with a single command on your control node (your computer).

Open your terminal.

Run the following command:

```
ssh-keygen -t rsa -b 4096
```

* ssh-keygen: This is the command-line tool for creating new SSH keys.

* -t rsa: The -t flag stands for "type". We are specifying the rsa algorithm, which is a common and secure standard.

* -b 4096: The -b flag stands for "bits". We are specifying a key length of 4096 bits, which is very secure.


* Enter file in which to save the key (/home/saman/.ssh/id_rsa): Just press Enter to accept the default location.

* Enter passphrase (empty for no passphrase): For automation with Ansible, it is strongly recommended to not use a passphrase. Just press Enter to leave it empty.

* Enter same passphrase again: Press Enter again.

This process creates two files in your ~/.ssh/ directory:

**`id_rsa`**: Your private key. Never share this file with anyone.

**`id_rsa.pub`**: Your public key. This is the key you will copy to other servers.

### Step 1.2: How to Copy Your Public Key to a Server
Now, you need to copy your public key to each server you want to manage with Ansible. The easiest and most recommended way is using the ssh-copy-id command.

Run the following command for each of your servers, replacing user with your username on the server and remote_host_ip with the server's IP address.

```
ssh-copy-id user@remote_host_ip
```

* ssh-copy-id: This tool automatically copies your public key to the remote server and places it in the correct file (~/.ssh/authorized_keys) with the correct permissions.

* user@remote_host_ip: The username and IP address of the server you want to give access to.

You will be asked for your password for user@remote_host_ip one last time. After you enter it, the key will be copied.

### Step 1.3: How to SSH to the Server
After copying the key, you can now connect to your server without a password.

```
ssh user@remote_host_ip
```

If everything was done correctly, you should be logged in immediately without being asked for a password. Repeat Step 1.2 for all servers you intend to manage.

## Part 2: How to Write the hosts.yml File
The inventory file is the foundation of Ansible. It tells Ansible what servers exist and how to connect to them. Using the YAML format (hosts.yml) is the modern standard.

Here is a sample hosts.yml file. Let's write it and then explain it word-for-word.

File: hosts.yml

```
all:
  children:
    webservers:
      hosts:
        web1:
          ansible_host: 192.168.1.10
        web2:
          ansible_host: 192.168.1.11
    dbservers:
      hosts:
        db1:
          ansible_host: 192.168.1.20
  vars:
    ansible_user: saman
    ansible_ssh_private_key_file: ~/.ssh/id_rsa
```

**`all:`**: This is a special, built-in group in Ansible that automatically contains every host you define in the inventory. It acts as the top-level container for your entire infrastructure.

**`children:`**: This keyword indicates that you are defining groups that are "children" of the parent group (in this case, all). This is the standard way to create your own custom groups.

**`webservers:`**: This is a custom group name you have chosen. Its purpose is to logically organize all your web servers. In a playbook, you can choose to run tasks only against the webservers group.

**`hosts:`**: This keyword, nested under a group name, signifies the beginning of the list of individual servers (hosts) that belong to that group.

**`web1:`**: This is a friendly alias or logical name for your first web server. This is the name you will use to refer to this specific server. It can be anything you want.

**`ansible_host`**: 192.168.1.10: This is a critical Ansible Connection Variable. It tells Ansible the actual IP address (or DNS name) it should use to connect to the server you've named web1.

**`dbservers:`**: This is another custom group name, created to organize your database servers separately from your web servers.

**`vars:`**: This keyword is used to define variables that will apply to the group it is defined under. Since it is under all, the variables here will apply to every single host in your inventory.

**`ansible_user`**: saman: This variable sets the default username that Ansible will use for SSH connections to all hosts. You can override this for specific hosts or groups if needed.

**`ansible_ssh_private_key_file`**: ~/.ssh/id_rsa: This variable tells Ansible the exact path to the private SSH key it should use for authentication. This should match the private key you generated in Part 1.

## Part 3: How to Write a Playbook
A playbook is where you define the steps to automate a process. It's a YAML file containing a list of "plays," and each play contains a list of "tasks."

Here is a sample playbook that installs and starts the Nginx web server.

File: install_nginx.yml
```
---
- name: Install and Start Nginx
  hosts: webservers
  become: yes
  tasks:
    - name: Install the latest version of Nginx
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Start and enable the Nginx service
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: yes
```

**`---`**: This is standard YAML syntax that optionally indicates the start of a document. It's a good practice to include it.

**`- (hyphen)`**: In YAML, a hyphen signifies the start of a list item. An Ansible playbook is a list of one or more "plays." This file contains a single play.

**`name`**: Install and Start Nginx: This is a human-readable description for the entire play. This name will be printed on the screen when the playbook runs, helping you understand what's happening.

**`hosts`**: webservers: This is the target section. It tells Ansible to run this entire play only on the hosts that belong to the webservers group in your hosts.yml inventory file.

**`become`**: yes: This is for privilege escalation. Setting it to yes instructs Ansible to use sudo (by default) to execute the tasks. This is necessary for system-level operations like installing software or managing services.

**`tasks:`**: This keyword marks the beginning of the list of actions to be performed in this play. The tasks listed under it are executed in order, from top to bottom.

**`- name`**: Install the latest version of Nginx: This is the description for a single task. Like the play name, this is printed during execution, so you can track the playbook's progress step-by-step.

**`ansible.builtin.apt:`**: This is the module being called for the task.

A module is a reusable, standalone script that Ansible runs. The apt module is used to manage packages on Debian-based systems like Ubuntu.

ansible.builtin is the collection where this core module resides. Using the full name (Fully Qualified Collection Name or FQCN) is a modern best practice.

**`name: nginx:`** This is an argument (or parameter) passed to the apt module. It specifies the name of the package to be managed.

**`state`**: present: This is another argument for the apt module. The state present ensures that the package is installed. If it's already installed, Ansible does nothing, thanks to its idempotent nature.

**`update_cache`**: yes: This argument tells the apt module to run the equivalent of apt-get update before attempting to install the package. This ensures the local package index is up-to-date.

**`ansible.builtin.service:`**: This calls the service module, which is used to control system services (e.g., start, stop, restart).

**`name`**: nginx: An argument specifying the name of the service to manage.

**`state`**: started: This argument ensures that the Nginx service is running.

**`enabled: yes`**: This argument ensures that the Nginx service is configured to start automatically when the server boots up.

## Part 4: How to Run the Playbook (The Command)
Now that you have your inventory (hosts.yml) and your playbook (install_nginx.yml), you can run it with the following command:
```
ansible-playbook -i hosts.yml install_nginx.yml
```

**`ansible-playbook`**: This is the Ansible command used specifically to execute playbook files. It reads the playbook and orchestrates the tasks defined within it.

**`-i`**: This is a flag (or option), short for --inventory. It tells the ansible-playbook command where to find your inventory file.

**`hosts.yml`**: This is the value for the -i flag. You are providing the path to your inventory file.

**`install_nginx.yml`**: This is the final argument to the command. It is the path to the playbook file that you want to execute

### Some Practices

#### Practice 1: Add an Alias to a User's .bashrc File
Question: How can you add a permanent alias (like mk for mkdir) to a specific user's .bashrc file?

```
- name: Add alias
  hosts: devops-test
  tasks:
    - name: Add mk alias
      lineinfile:
        path: /home/saman/.bashrc
        line: alias mk='mkdir'
```

#### Practice 2: Install Multiple Packages Using a Loop
Question: How can you install a list of packages (like curl, btop, git, apache2) using a loop within a single task?
```
- name: Install packages
  hosts: devops-test
  become: true
  tasks:
    - name: Install curl and btop
      apt:
        name: "{{ item }}"
      loop:
        - curl
        - btop
        - git
        - apache2
```

#### Practice 3: Create a New User
Question: How can you ensure that a new user named devops exists on the system?
```
- name: create user
  hosts: devops-test
  become: yes
  tasks:
    - name: add user
      user:
        name: devops
        state: present
```

#### Practice 4: Create a Cron Job to Run a Script
Question: How can you create a cron job that runs a script named cron.sh every two hours, after first copying the script to the remote host?

```
- name: Create cron job
  hosts: devops-test
  become: true
  tasks:
    - name: Create a job
      cron:
        name: "My test job"
        job: "./cron.sh"
        hour: "*/2****"
    - name: touch file 
      copy:
        src: "./cron.sh"
        dest: "/home/saman/"
```

#### Practice 5: Install and Then Remove a Package
Question: How can you ensure the git package is installed and then subsequently remove it within the same playbook?
```
- name: Install and remove git
  hosts: devops-test
  become: true
  tasks:
    - name: Install git
      apt:
        name: git
        state: present

    - name: Remove git
      apt:
        name: git
        state: absent
```

#### Practice 6: Change the System's Hostname
Question: How can you change the system's hostname to devops-sami?
```
- name: hostname
  hosts: devops-test
  become: true
  tasks:
    - name: set hostname
      hostname:
        name: devops-sami
```

#### Practice 7: Add an Entry to the /etc/hosts File
Question: How can you add a new line to the /etc/hosts file using the shell module?
```
- name: Add to hosts file
  hosts: devops-test
  become: true
  tasks:
    - name: Add devops.com
      shell: |
        sed -i "6i 192.168.114.216 devops222.com" /etc/hosts
```

#### Practice 8: Display a Gathered Fact (IP Address)
Question: How can you display a gathered fact, such as the host's IPv6 addresses, using the debug module?
```
- name: Get IP
  hosts: devops-test
  tasks:
    - name: Show IP address
      debug:
        msg: "{{ ansible_all_ipv6_addresses }}"
```

#### Practice 9: Create a New Directory
Question: How can you create a new directory at the path /opt/home/devops-happy?
```
- name: Create directory
  hosts: devops-test
  become: true
  tasks:
    - name: Create the directory
      file:
        path: /opt/home/devops-happy
        state: directory
```

#### Practice 10: Change File Permissions
Question: How can you change the permissions of a file named hello.txt to 644?
```
- name: permission
  hosts: devops-test
  become: true
  tasks:
    - name: change
      file:
        path: "/home/saman/hello.txt"
        mode: 644
```

#### Practice 11: Manage the Firewall (UFW) and Deny a Port
Question: How can you use the ufw module to create a rule to deny traffic on port 8080?
```
- name: Manage firewall
  hosts: devops-test
  become: TRUE
  tasks:
    - name: Deny port 80
      ufw:
        rule: deny
        port: '8080'
```

#### Practice 12: Change the Root User's Password
Question: How can you update the root user's password?
```
- name: Change root password
  hosts: devops-test
  become: true
  tasks:
    - name: Update root password
      user:
        name: root
        password: 1234
```

Security Note: Storing passwords in plaintext within a playbook is a major security risk. In real-world scenarios, always use Ansible Vault to encrypt sensitive data.

#### Practice 13: Execute a Local Script on a Remote Host
Question: How can you execute a local script (hello.sh) from the control node on the remote host?
```
- name: Execute script
  hosts: devops-test
  tasks:
    - name: Run hello.sh
      script: ./hello.sh
```

#### Practice 14: Read the Content of a Remote File
Question: How can you read the content of a remote file (/etc/hostname), store it in a variable, and then display it?
```
- name: Read file
  hosts: devops-test
  tasks:
    - name: Read /etc/hostname
      slurp:
        src: /etc/hostname
      register: file_content

    - name: Show content
      debug:
        msg: "{{ file_content['content'] | b64decode }}"
```

### Templating with Jinja2
In Ansible, you don't just work with static files. You often need to create dynamic content, like configuration files that change based on which server you are targeting. This is where Jinja2 comes in.

Jinja2 is a powerful templating engine for Python. Ansible uses it to allow you to insert variables, run loops, and apply conditional logic directly inside your files.

#### Why Use Jinja2?
Imagine you need to deploy a web application configuration file. The server_name might be prod.example.com in production but dev.example.com in development. Instead of maintaining two separate files, you can create one template and dynamically insert the correct server name based on a variable.

#### Jinja2 Syntax
Jinja2 has a few simple but powerful syntax elements you'll use constantly in Ansible:

##### 1. Expressions: {{ ... }}
An expression is used to print the value of a variable into the file. Anything inside the double curly braces will be replaced by the value of that variable when the playbook runs.

Example Playbook:
```
- hosts: webservers
  vars:
    package_name: "nginx"
  tasks:
    - name: Install a package
      ansible.builtin.apt:
        name: "{{ package_name }}" # The value of package_name will be inserted here
        state: present
```

##### 2. Statements: {% ... %}
A statement is used for logic, such as if/else conditions or for loops. This logic is executed when the template is rendered, but it doesn't print anything directly into the final file (unless it contains expressions).

Example Playbook:
```
- hosts: all
  tasks:
    - name: Show a custom message
      ansible.builtin.debug:
        msg: |
          {% if ansible_hostname == "web1" %}
            This is the primary web server.
          {% else %}
            This is a different server.
          {% endif %}
```

### Using the template Module
The most common way to use Jinja2 is with the template module. This module takes a template file (ending in .j2) from your control node, processes it with Jinja2, and copies the resulting file to the managed node.

#### Step 1: Create a template file (nginx.conf.j2)

This file looks like a normal Nginx config file, but with a variable for the number of worker processes.

Code snippet
```
/path/to/templates/nginx.conf.j2

user www-data;
worker_processes {{ ansible_processor_vcpus }}; # This will be replaced by a fact
pid /run/nginx.pid;

events {
  worker_connections 768;
}

# ... rest of the config
```
* {{ ansible_processor_vcpus }} is an Ansible "fact"—a piece of data Ansible automatically discovers about the server.

#### Step 2: Use the template module in your playbook
```
- name: Deploy Nginx config from template
  hosts: webservers
  become: yes
  tasks:
    - name: Copy templated Nginx config file
      ansible.builtin.template:
        src: /path/to/templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
```

When this runs, Ansible will read nginx.conf.j2, replace {{ ansible_processor_vcpus }} with the actual number of CPU cores on the target server, and save the final file as /etc/nginx/nginx.conf.

### Ansible Facts
Before a playbook runs any tasks on a host, it first performs a step called Gathering Facts.

### Gathering Facts
This is an automatic process where Ansible connects to a managed node and collects a huge amount of information (facts) about it. This includes details like:

* Hardware information (CPU, memory, disk space)

* Operating system and version

* Network interfaces and IP addresses

* Hostname

All this information is stored in built-in variables that you can use in your playbooks, just like {{ ansible_hostname }} or {{ ansible_processor_vcpus }}. This allows you to write playbooks that adapt to different systems.

### Local Facts
Sometimes, the automatically gathered facts are not enough. You might have custom information about a server that you want Ansible to know, such as its role (e.g., "web_server_prod") or the data_center it belongs to.

Local Facts allow you to define your own custom facts on a managed node.

#### How it works:

On the managed node (your server), create the directory /etc/ansible/facts.d.

Inside that directory, create a file ending in .fact. The file should be in INI or JSON format and contain your custom key-value data.

Example server_info.fact file on a managed node:
```
[general]
role=web_server_prod
data_center=london
```

When Ansible runs and gathers facts, it will find this file and load its content into the ansible_local variable. You can then access it in your playbook like this:
```
- name: Display local facts
  hosts: all
  tasks:
    - name: Show the server role
      ansible.builtin.debug:
        msg: "This server's role is {{ ansible_local.general.role }}."
```

### Working with Variables and Logic
Variables and logic are what make playbooks truly dynamic and powerful.

Defining Variables in a Playbook
The simplest way to define a variable is directly within your playbook using the vars keyword. These variables can then be used anywhere in the play.
```
- name: Deploy Apache Web Server
  hosts: webservers
  become: true
  vars:
    apache_package: httpd
    apache_service: httpd
  tasks:
    - name: Install Apache
      ansible.builtin.yum:
        name: "{{ apache_package }}"
        state: present

    - name: Start Apache service
      ansible.builtin.service:
        name: "{{ apache_service }}"
        state: started
```

#### Registering Task Output as a Variable
Sometimes, you need to use the output of one task as an input for another. The register keyword saves the entire output of a task (including whether it succeeded, failed, and its return values) into a new variable.
```
- name: Check if a file exists
  hosts: all
  tasks:
    - name: Use 'stat' to check for file
      ansible.builtin.stat:
        path: /etc/motd
      register: motd_file

    - name: Show the registered variable
      ansible.builtin.debug:
        var: motd_file
```

The motd_file variable will now be a complex JSON object containing details about the file. For example, motd_file.stat.exists will be true or false.

#### Conditional Execution with when
The when statement allows you to run a task only if a certain condition is met. This is often used with variables created by register.

Let's expand on the previous example: install a package only if a command fails.
```
- name: Install Nginx only if it is not installed
  hosts: all
  become: yes
  tasks:
    - name: Check if nginx command exists
      ansible.builtin.command: "which nginx"
      register: nginx_check
      ignore_errors: yes # Important: Prevents the play from failing if nginx is not found
      changed_when: false # Important: This command doesn't change anything, so don't report it as "changed"

    - name: Install nginx if it was not found
      ansible.builtin.apt:
        name: nginx
        state: present
      when: nginx_check.rc != 0 # .rc is the return code. 0 means success, non-zero means failure.
```

This playbook will only run the "Install nginx" task if the which nginx command failed (i.e., returned a code other than 0).

### Loops
A loop lets you run a task multiple times with different values. This is extremely useful for repetitive actions like creating multiple users or installing a list of packages.
```
- name: Install multiple development packages
  hosts: all
  become: yes
  tasks:
    - name: Install packages using a loop
      ansible.builtin.apt:
        name: "{{ item }}"
        state: present
      loop:
        - vim
        - git
        - curl
        - htop
```

In this example, the task will run four times. Each time, the special variable {{ item }} will be replaced with the next value from the loop (vim, then git, etc.).

### Using Tags to Control Execution
In a large playbook, you may not always want to run every single task. Tags allow you to label plays or individual tasks so you can selectively run only the parts you need.

Example Playbook with Tags:
```
- name: Full server setup
  hosts: all
  become: yes
  tasks:
    - name: Update all packages
      ansible.builtin.apt:
        upgrade: dist
      tags:
        - packages

    - name: Create a new user
      ansible.builtin.user:
        name: devops
        state: present
      tags:
        - users

    - name: Install Nginx
      ansible.builtin.apt:
        name: nginx
        state: present
      tags:
        - packages
        - web
```

Now you can control execution from the command line:

Run only the tasks for installing/updating packages:
```
ansible-playbook my_server_setup.yml --tags packages
```

Run only the web-related tasks:
```
ansible-playbook my_server_setup.yml --tags web
```

Skip a specific tag (e.g., run everything EXCEPT user creation):
```
ansible-playbook my_server_setup.yml --skip-tags users
```
Of course. Let's proceed with the conceptual explanation of Ansible Roles. Here is the documentation covering what they are, their benefits, and a detailed, formal description of the standard directory structure, without providing code examples.

## Ansible Roles
### What Are Roles and Why Are They Used?
In Ansible, a Role is a standardized and self-contained structure for packaging and reusing automation content. It is the primary mechanism for breaking down large, complex playbooks into smaller, more manageable components. Think of a Role as a portable, reusable "mini-playbook" designed to perform a specific function, such as installing and configuring a web server, setting up a database, or deploying an application.

The core purpose of using Roles is to abstract complexity and promote a modular approach to automation. Instead of writing a single, monolithic playbook that performs hundreds of steps, you can encapsulate related tasks, variables, files, and templates into a distinct Role. A master playbook then simply calls upon one or more Roles to execute a larger workflow.

### Advantages of Using Roles
The adoption of a Role-based structure provides several significant benefits for managing automation projects:

* Reusability: A Role created for one project can be easily reused in another. For example, a well-written nginx Role can be used to deploy a web server in any project, saving time and effort.

* Organization and Readability: Roles enforce a logical structure on your automation code. This makes playbooks cleaner, shorter, and easier to read, as the main playbook becomes a high-level overview of the workflow, rather than a long list of individual tasks.

* Maintainability and Testability: By isolating functionality, Roles become easier to maintain, test, and debug. An issue with a web server configuration can be addressed within its specific Role without affecting the logic for the database or other components.

* Shareability and Collaboration: Roles are designed to be shared. They can be version-controlled in their own repositories and shared among team members or with the public community through platforms like Ansible Galaxy. This fosters collaboration and allows teams to leverage pre-built automation for common tasks.

### The Standard Role Directory Structure
Ansible automatically recognizes a predefined directory structure for Roles. When you organize your content according to this structure, Ansible knows where to find the relevant tasks, variables, and other files without explicit path declarations in your playbook.

A Role is, at its core, a directory containing several standard subdirectories. The main ones are:

#### tasks/
This is the most critical directory within a Role. It contains the main list of tasks to be executed. Ansible looks for a file named main.yml inside this directory to serve as the entry point for the Role's tasks. All other task files within this directory must be included from the tasks/main.yml file.

#### handlers/
This directory is used to store handlers. Handlers are special tasks that are only executed when "notified" by another task. They are typically used for actions that should only occur if a change has been made, such as restarting a service after its configuration file has been updated. The main file for handlers is handlers/main.yml.

#### defaults/
This directory contains the default variables for the Role. The variables defined here have the lowest precedence of all variable sources. The purpose of this directory is to provide sensible, default values for the Role's variables, which can be easily overridden by the user in their playbook or inventory. The main file for these variables is defaults/main.yml.

#### vars/
This directory is also used for defining variables for the Role. However, variables defined here have a high precedence and are intended for internal use within the Role. They are not easily overridden by the user. The main file for these variables is vars/main.yml.

#### files/
This directory is for storing static files that the Role may need to deploy to a managed node. These files are deployed using the copy module and are transferred without any modification. For example, you might place a pre-written script or a binary file here.

#### templates/
This directory is for storing template files, which typically have a .j2 extension. Unlike the files directory, these files are processed by the Jinja2 templating engine before being deployed to the managed node. This allows you to use variables, loops, and other logic to create dynamic content. Templates are deployed using the template module.

#### meta/
This directory contains metadata about the Role itself. The primary file, meta/main.yml, defines information such as the author, license, supported platforms, and, most importantly, role dependencies. If your Role requires another Role to be executed first, you define that dependency here. This information is also used by Ansible Galaxy.