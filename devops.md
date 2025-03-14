# Devops

Topic of this Course:

* 1-http/https,Nginx

* 2-Docker

* 3-Git/Gitlab CI

* 4-Bash scripting

* 5-Ansible

* 5-Prometheus/Grafana

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
Example Dockerfile for a Python app:

#### 5. Docker Hub:
This is a big online library (like a cloud storage for Docker images) where people share pre-made images. Want a container with MySQL or Node.js? You can download an image from Docker Hub and start a container in seconds.
You can also upload your own images to share with others.

#### 6. Docker Registry:
A storage system for Docker images, allowing you to save, share, and distribute them.
Key details:
Public Registries: Docker Hub is the most famous one, offering free public image hosting (e.g., docker pull nginx gets an image from Docker Hub).
Private Registries: You can set up your own registry (e.g., on your server or cloud) for private images using Docker’s open-source registry software or services like Amazon ECR, Google Container Registry, or GitHub Container Registry.
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

### 
