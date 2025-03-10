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