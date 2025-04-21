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